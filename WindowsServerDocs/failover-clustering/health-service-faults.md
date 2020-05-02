---
title: Errores de Servicio de mantenimiento
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 5fe2f98c89d97325c1f59dc6ba292831e0ffa5ff
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720560"
---
# <a name="health-service-faults"></a>Errores de Servicio de mantenimiento

> Se aplica a: Windows Server 2019, Windows Server 2016

## <a name="what-are-faults"></a>Qué son los errores

El Servicio de mantenimiento supervisa constantemente el clúster de Espacios de almacenamiento directo para detectar problemas y generar "errores". Un nuevo cmdlet muestra todos los errores actuales, lo que le permite comprobar fácilmente el estado de la implementación sin examinar cada entidad o característica a su vez. Los errores están diseñados para ser precisos, fáciles de entender y accionable.  

Cada error contiene cinco campos importantes:  

-   severity
-   Descripción del problema
-   Pasos siguientes recomendados para solucionar el problema
-   Información de identificación de la entidad con errores
-   Su ubicación física (si procede)

Por ejemplo, este es un error habitual:  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > La ubicación física procede de la configuración del dominio de error. Para obtener más información acerca de los dominios de error, consulte [dominios de error en Windows Server 2016](fault-domains.md). Si no proporciona esta información, el campo de ubicación será menos útil, por ejemplo, puede mostrar solo el número de ranura.  

## <a name="root-cause-analysis"></a>Análisis de la causa raíz

El Servicio de mantenimiento puede evaluar la causalidad potencial entre las entidades con errores para identificar y combinar los errores que son consecuencias del mismo problema subyacente. Al reconocer las cadenas de efectos, habrá menos informes innecesarios. Por ejemplo, si un servidor está inactivo, se espera que las unidades del servidor también estén sin conectividad. Por lo tanto, solo se generará un error para la causa raíz; en este caso, el servidor.  

## <a name="usage-in-powershell"></a>Uso en PowerShell

Para ver los errores actuales de PowerShell, ejecute este cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Esto devuelve cualquier error que afecte al clúster global de Espacios de almacenamiento directo. A menudo, estos errores se relacionan con el hardware o la configuración. Si no hay ningún error, este cmdlet no devolverá nada.  

>[!NOTE]
> En un entorno que no sea de producción y bajo su propio riesgo, puede experimentar con esta característica desencadenando los errores, por ejemplo, quitando un disco físico o apagando un nodo. Una vez que haya aparecido el error, vuelva a insertar el disco físico o reinicie el nodo y el error desaparecerá de nuevo.

También puede ver los errores que solo afectan a los volúmenes o recursos compartidos de archivos específicos con los siguientes cmdlets:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Esto devuelve los errores que afectan solo al volumen específico o al recurso compartido de archivos. A menudo, estos errores están relacionados con el planeamiento de la capacidad, la resistencia de los datos o características como la calidad de servicio o la réplica de almacenamiento. 

## <a name="usage-in-net-and-c"></a>Uso en .NET y C #

### <a name="connect"></a>Conectar

Para consultar el Servicio de mantenimiento, tendrá que establecer un **CimSession** con el clúster. Para ello, necesitará algunas cosas que solo están disponibles en .NET completo, lo que significa que no puede hacerlo directamente desde una aplicación web o móvil. Estos ejemplos de código usarán\#C, la opción más sencilla para esta capa de acceso a datos.

```
using System.Security;
using Microsoft.Management.Infrastructure;

public CimSession Connect(string Domain = "...", string Computer = "...", string Username = "...", string Password = "...")
{
    SecureString PasswordSecureString = new SecureString();
    foreach (char c in Password)
    {
        PasswordSecureString.AppendChar(c);
    }

    CimCredential Credentials = new CimCredential(
        PasswordAuthenticationMechanism.Default, Domain, Username, PasswordSecureString);
    WSManSessionOptions SessionOptions = new WSManSessionOptions();
    SessionOptions.AddDestinationCredentials(Credentials);
    Session = CimSession.Create(Computer, SessionOptions);
    return Session;
}
```

El nombre de usuario proporcionado debe ser un administrador local del equipo de destino.

Se recomienda que construya la contraseña **SecureString** directamente a partir de los datos proporcionados por el usuario en tiempo real, por lo que su contraseña nunca se almacena en la memoria en texto no cifrado. Esto ayuda a mitigar una gran variedad de problemas de seguridad. Pero en la práctica, la construcción de lo anterior es común para la creación de prototipos.

### <a name="discover-objects"></a>Detectar objetos

Con el **CimSession** establecido, puede consultar instrumental de administración de Windows (WMI) en el clúster.

Antes de que pueda obtener errores o métricas, deberá obtener instancias de varios objetos pertinentes. En primer lugar, el **StorageSubSystem de msft\_** que representa espacios de almacenamiento directo en el clúster. Con esto, puede obtener todos los **StorageNode\_de msft** del clúster y todos los **volúmenes\_msft**, los volúmenes de datos. Por último, también necesitará **el\_StorageHealth de msft**, el propio servicio de mantenimiento.

```
CimInstance Cluster;
List<CimInstance> Nodes;
List<CimInstance> Volumes;
CimInstance HealthService;

public void DiscoverObjects(CimSession Session)
{
    // Get MSFT_StorageSubSystem for Storage Spaces Direct
    Cluster = Session.QueryInstances(@"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageSubSystem")
        .First(Instance => (Instance.CimInstanceProperties["FriendlyName"].Value.ToString()).Contains("Cluster"));

    // Get MSFT_StorageNode for each cluster node
    Nodes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageNode", null, "StorageSubSystem", "StorageNode").ToList();

    // Get MSFT_Volumes for each data volume
    Volumes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToVolume", null, "StorageSubSystem", "Volume").ToList();

    // Get MSFT_StorageHealth itself
    HealthService = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageHealth", null, "StorageSubSystem", "StorageHealth").First();
}
```

Estos son los mismos objetos que se obtienen en PowerShell mediante cmdlets como **Get-StorageSubSystem**, **Get-StorageNode**y **Get-Volume**.

Puede tener acceso a todas las mismas propiedades, documentadas en [clases de API de administración de almacenamiento](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx).

```
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

### <a name="query-faults"></a>Errores de consulta

Invocar **diagnóstico** para obtener cualquier error actual cuyo ámbito sea el **CimInstance**de destino, que es el clúster o cualquier volumen.

A continuación se describe la lista completa de errores disponibles en cada ámbito en Windows Server 2016.

```       
public void GetFaults(CimSession Session, CimInstance Target)
{
    // Set Parameters (None)
    CimMethodParametersCollection FaultsParams = new CimMethodParametersCollection();
    // Invoke API
    CimMethodResult Result = Session.InvokeMethod(Target, "Diagnose", FaultsParams);
    IEnumerable<CimInstance> DiagnoseResults = (IEnumerable<CimInstance>)Result.OutParameters["DiagnoseResults"].Value;
    // Unpack
    if (DiagnoseResults != null)
    {
        foreach (CimInstance DiagnoseResult in DiagnoseResults)
        {
            // TODO: Whatever you want!
        }
    }
}
```

### <a name="optional-myfault-class"></a>Opcional: subfault (clase)

Puede que le resulte útil construir y conservar su propia representación de errores. Por ejemplo, esta clase de **error** almacena varias propiedades clave de errores, como **FaultId**, que se pueden usar más adelante para asociar la actualización o quitar notificaciones, o para desduplicar en caso de que se detecte el mismo error varias veces, por cualquier motivo.

```       
public class MyFault {
    public String FaultId { get; set; }
    public String Reason { get; set; }
    public String Severity { get; set; }
    public String Description { get; set; }
    public String Location { get; set; }

    // Constructor
    public MyFault(CimInstance DiagnoseResult)
    {
        CimKeyedCollection<CimProperty> Properties = DiagnoseResult.CimInstanceProperties;
        FaultId     = Properties["FaultId"                  ].Value.ToString();
        Reason      = Properties["Reason"                   ].Value.ToString();
        Severity    = Properties["PerceivedSeverity"        ].Value.ToString();
        Description = Properties["FaultingObjectDescription"].Value.ToString();
        Location    = Properties["FaultingObjectLocation"   ].Value.ToString();
    }
}
```

```
List<MyFault> Faults = new List<MyFault>;

foreach (CimInstance DiagnoseResult in DiagnoseResults)
{
    Faults.Add(new Fault(DiagnoseResult));
}
```

A continuación se describe la lista completa de las propiedades de cada error (**DiagnoseResult**).

### <a name="fault-events"></a>Eventos de error

Cuando se crean, quitan o actualizan errores, el Servicio de mantenimiento genera eventos WMI. Son esenciales para mantener el estado de la aplicación sincronizada sin sondeos frecuentes y pueden ayudar con cosas como determinar cuándo enviar alertas por correo electrónico, por ejemplo. Para suscribirse a estos eventos, este código de ejemplo utiliza de nuevo el modelo de diseño de observador.

En primer lugar, suscríbase a los eventos **msft\_StorageFaultEvent** .

```      
public void ListenForFaultEvents()
{
    IObservable<CimSubscriptionResult> Events = Session.SubscribeAsync(
        @"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageFaultEvent");
    // Subscribe the Observer
    FaultsObserver<CimSubscriptionResult> Observer = new FaultsObserver<CimSubscriptionResult>(this);
    IDisposable Disposeable = Events.Subscribe(Observer);
}   
```

A continuación, implemente un observador cuyo método **Next ()** se invocará cada vez que se genere un nuevo evento.

Cada evento contiene **ChangeType** que indica si se está creando, quitando o actualizando un error, y los **FaultId**pertinentes.

Además, contienen todas las propiedades del propio error.

```
class FaultsObserver : IObserver
{
    public void OnNext(T Event)
    {
        // Cast
        CimSubscriptionResult SubscriptionResult = Event as CimSubscriptionResult;

        if (SubscriptionResult != null)
        {
            // Unpack            
            CimKeyedCollection<CimProperty> Properties = SubscriptionResult.Instance.CimInstanceProperties;
            String ChangeType = Properties["ChangeType"].Value.ToString();
            String FaultId = Properties["FaultId"].Value.ToString();

            // Create
            if (ChangeType == "0")
            {
                Fault MyNewFault = new MyFault(SubscriptionResult.Instance);
                // TODO: Whatever you want!
            }
            // Remove
            if (ChangeType == "1")
            {
                // TODO: Use FaultId to find and delete whatever representation you have...
            }
            // Update
            if (ChangeType == "2")
            {
                // TODO: Use FaultId to find and modify whatever representation you have...
            }
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Nothing
    }
}
```

### <a name="understand-fault-lifecycle"></a>Descripción del ciclo de vida de los errores

Los errores no están diseñados para ser marcados como "vistos" o resueltos por el usuario. Se crean cuando el Servicio de mantenimiento observa un problema y se quitan automáticamente y solo cuando el Servicio de mantenimiento ya no puede seguir el problema. En general, esto refleja que se ha corregido el problema.

Sin embargo, en algunos casos, el Servicio de mantenimiento puede volver a detectar los errores (por ejemplo, después de la conmutación por error o debido a una conectividad intermitente, etc.). Por este motivo, puede que tenga sentido conservar su propia representación de errores, por lo que puede desduplicar fácilmente. Esto es especialmente importante si se envían alertas por correo electrónico o equivalentes.

### <a name="properties-of-faults"></a>Propiedades de los errores

Esta tabla presenta varias propiedades clave del objeto de error. En el esquema completo, inspeccione la clase **msft\_StorageDiagnoseResult** en *storagewmi. mof*.

| **Propiedad**              | **Ejemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft. Health. FaultType. VOLUME. Capacity                      |
| Motivo                    | "El volumen se está quedando sin espacio disponible".                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | S.N. XYZ9000 contoso 123456789                                  |
| FaultingObjectLocation    | Bastidor A06, RU 25, ranura 11                                        |
| RecommendedActions        | {"Expandir el volumen.", "migrar cargas de trabajo a otros volúmenes"}   |

**FaultId** Único dentro del ámbito de un clúster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"informativo", "WARNING" y "error"}, o colores equivalentes, como azul, amarillo y rojo.

**FaultingObjectDescription** Información de elementos para hardware, normalmente en blanco para objetos de software.

**FaultingObjectLocation** Información de ubicación para el hardware, normalmente en blanco para objetos de software.

**RecommendedActions** Lista de acciones recomendadas, que son independientes y no tienen ningún orden determinado. Actualmente, esta lista tiene una longitud de 1 a menudo.

## <a name="properties-of-fault-events"></a>Propiedades de los eventos de error

Esta tabla presenta varias propiedades clave del evento de error. En el esquema completo, inspeccione la clase **msft\_StorageFaultEvent** en *storagewmi. mof*.

Tenga en cuenta el **ChangeType**, que indica si se está creando, quitando o actualizando un error, y **FaultId**. Un evento también contiene todas las propiedades del error afectado.

| **Propiedad**              | **Ejemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft. Health. FaultType. VOLUME. Capacity                      |
| Motivo                    | "El volumen se está quedando sin espacio disponible".                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | S.N. XYZ9000 contoso 123456789                                  |
| FaultingObjectLocation    | Bastidor A06, RU 25, ranura 11                                        |
| RecommendedActions        | {"Expandir el volumen.", "migrar cargas de trabajo a otros volúmenes"}   |

**ChangeType** ChangeType = {0, 1, 2} = {"crear", "quitar", "actualizar"}.

## <a name="coverage"></a>Cobertura

En Windows Server 2016, el Servicio de mantenimiento proporciona la siguiente cobertura de errores:  

### <a name="physicaldisk-8"></a>**DiscoFísico (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft. Health. FaultType. DiscoFísico. FailedMedia
* Gravedad: advertencia
* Motivo: *"error en el disco físico".*
* RecommendedAction: *"reemplazar el disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft. Health. FaultType. DiscoFísico. LostCommunication
* Gravedad: advertencia
* Motivo: *"se ha perdido la conectividad con el disco físico".*
* RecommendedAction: *"Compruebe que el disco físico funciona y está correctamente conectado".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft. Health. FaultType. DiscoFísico. no responde
* Gravedad: advertencia
* Motivo: *"el disco físico está exhibiendo no responde de forma periódica".*
* RecommendedAction: *"reemplazar el disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft. Health. FaultType. DiscoFísico. PredictiveFailure
* Gravedad: advertencia
* Motivo: *"no se prevé que se produzca un error del disco físico en breve".*
* RecommendedAction: *"reemplazar el disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft. Health. FaultType. DiscoFísico. UnsupportedHardware
* Gravedad: advertencia
* Motivo: *"el disco físico está en cuarentena porque no lo admite el proveedor de la solución".*
* RecommendedAction: *"reemplazar el disco físico por el hardware compatible".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft. Health. FaultType. DiscoFísico. UnsupportedFirmware
* Gravedad: advertencia
* Motivo: *"el disco físico está en cuarentena porque su versión de firmware no es compatible con el proveedor de la solución".*
* RecommendedAction: *"actualizar el firmware en el disco físico a la versión de destino".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft. Health. FaultType. DiscoFísico. UnrecognizedMetadata
* Gravedad: advertencia
* Motivo: *"el disco físico tiene metadatos no reconocidos".*
* RecommendedAction: *"este disco puede contener datos de un grupo de almacenamiento desconocido. En primer lugar, asegúrese de que no hay datos útiles en este disco y, a continuación, restablezca el disco ".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft. Health. FaultType. DiscoFísico. FailedFirmwareUpdate
* Gravedad: advertencia
* Motivo: *"error al intentar actualizar el firmware en el disco físico".*
* RecommendedAction: *"intentar usar un archivo binario de firmware diferente".*

### <a name="virtual-disk-2"></a>**Disco virtual (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft. Health. FaultType. VirtualDisks. NeedsRepair
* Gravedad: informativo
* Motivo: *"algunos datos de este volumen no son totalmente resistentes. Sigue siendo accesible ".*
* RecommendedAction: *"restaurando la resistencia de los datos".*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft. Health. FaultType. VirtualDisks. Detached
* Gravedad: crítico
* Motivo: *"no se puede obtener acceso al volumen. Es posible que se pierdan algunos datos ".*
* RecommendedAction: *"Compruebe la conectividad física o de red de todos los dispositivos de almacenamiento. Es posible que necesite restaurar desde la copia de seguridad ".*

### <a name="pool-capacity-1"></a>**Capacidad del grupo (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft. Health. FaultType. StoragePool. InsufficientReserveCapacityFault
* Gravedad: advertencia
* Motivo: *"el grupo de almacenamiento no tiene la capacidad de reserva mínima recomendada. Esto puede limitar la capacidad de restaurar la resistencia de los datos en caso de que se produzcan errores en la unidad.*
* RecommendedAction: *"agregar capacidad adicional al bloque de almacenamiento o liberar capacidad. La reserva recomendada mínima varía según la implementación, pero tiene aproximadamente 2 unidades de capacidad.*

### <a name="volume-capacity-2sup1sup"></a>**Capacidad del volumen (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft. Health. FaultType. VOLUME. Capacity
* Gravedad: advertencia
* Motivo: *"el volumen se está quedando sin espacio disponible".*
* RecommendedAction: *"expanda el volumen o migre las cargas de trabajo a otros volúmenes".*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft. Health. FaultType. VOLUME. Capacity
* Gravedad: crítico
* Motivo: *"el volumen se está quedando sin espacio disponible".*
* RecommendedAction: *"expanda el volumen o migre las cargas de trabajo a otros volúmenes".*

### <a name="server-3"></a>**Servidor (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft. Health. FaultType. Server. Down
* Gravedad: crítico
* Motivo: *"no se puede tener acceso al servidor".*
* RecommendedAction: *"iniciar o reemplazar servidor".*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft. Health. FaultType. Server. Isolated
* Gravedad: crítico
* Motivo: *"el servidor está aislado del clúster debido a problemas de conectividad".*
* RecommendedAction: *"si el aislamiento persiste, compruebe las redes o migre las cargas de trabajo a otros nodos".*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft. Health. FaultType. Server. Quarantine
* Gravedad: crítico
* Motivo: *"el servidor está en cuarentena por el clúster debido a errores recurrentes".*
* RecommendedAction: *"reemplazar el servidor o corregir la red".*

### <a name="cluster-1"></a>**Clúster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft. Health. FaultType. ClusterQuorumWitness. error
* Gravedad: crítico
* Motivo: *"el clúster es un error del servidor."*
* RecommendedAction: *"Compruebe el recurso de testigo y reinícielo según sea necesario. Iniciar o reemplazar servidores con errores ".*

### <a name="network-adapterinterface-4"></a>**Adaptador de red/interfaz (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft. Health. FaultType. adaptador de conexión. desconectado
* Gravedad: advertencia
* Motivo: *"la interfaz de red se ha desconectado".*
* RecommendedAction: *"volver a conectar el cable de red".*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft. Health. FaultType. interfaz cluster. Missing
* Gravedad: advertencia
* Motivo: *"el servidor {servidor} no tiene adaptadores de red conectados a la red de clústeres {red de clúster}."*
* RecommendedAction: *"conectar el servidor a la red de clústeres que faltan".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft. Health. FaultType. adaptador de hardware
* Gravedad: advertencia
* Motivo: *"se ha producido un error de hardware en la interfaz de red".*
* RecommendedAction: *"reemplazar el adaptador de la interfaz de red".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft. Health. FaultType. adaptador de. deshabilitado
* Gravedad: advertencia
* Motivo: *"la interfaz de red {interfaz de red} no está habilitada y no se está usando".*
* RecommendedAction: *"habilitar la interfaz de red".*

### <a name="enclosure-6"></a>**Gabinete (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. LostCommunication
* Gravedad: advertencia
* Motivo: *"la comunicación se ha perdido en el contenedor de almacenamiento".*
* RecommendedAction: *"iniciar o reemplazar el contenedor de almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. FanError
* Gravedad: advertencia
* Motivo: *"error en el ventilador de la posición {posición} del contenedor de almacenamiento".*
* RecommendedAction: *"reemplazar el ventilador en el contenedor de almacenamiento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. CurrentSensorError
* Gravedad: advertencia
* Motivo: *"error en el sensor actual en la posición {posición} del contenedor de almacenamiento".*
* RecommendedAction: *"reemplazar un sensor actual en el contenedor de almacenamiento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. VoltageSensorError
* Gravedad: advertencia
* Motivo: *"error en el sensor de voltaje en la posición {posición} del contenedor de almacenamiento".*
* RecommendedAction: *"reemplazar un sensor de voltaje en el contenedor de almacenamiento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. IoControllerError
* Gravedad: advertencia
* Motivo: *"error en el controlador de e/s en la posición {posición} del contenedor de almacenamiento".*
* RecommendedAction: *"reemplazar un controlador de e/s en el contenedor de almacenamiento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. TemperatureSensorError
* Gravedad: advertencia
* Motivo: *"error en el sensor de temperatura en la posición {posición} del contenedor de almacenamiento".*
* RecommendedAction: *"reemplazar un sensor de temperatura en el contenedor de almacenamiento."*

### <a name="firmware-rollout-3"></a>**Lanzamiento de firmware (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft. Health. FaultType. FaultDomain. FailedMaintenanceMode
* Gravedad: advertencia
* Motivo: *"actualmente no se puede realizar el progreso mientras se realiza la implementación del firmware".*
* RecommendedAction: *"Compruebe que todos los espacios de almacenamiento son correctos y que ningún dominio de error está actualmente en modo de mantenimiento".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft. Health. FaultType. FaultDomain. FirmwareVerifyVersionFaile
* Gravedad: advertencia
* Motivo: *"la puesta al día del firmware se canceló debido a la información de versión de firmware no leída o inesperada después de aplicar una actualización de firmware".*
* RecommendedAction: *"reiniciar el firmware cuando se ha resuelto el problema de firmware".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft. Health. FaultType. FaultDomain. TooManyFailedUpdates
* Gravedad: advertencia
* Motivo: *"la puesta al día del firmware se canceló debido a que hay demasiados discos físicos con errores en un intento de actualización de firmware".*
* RecommendedAction: *"reiniciar el firmware cuando se ha resuelto el problema de firmware".*

### <a name="storage-qos-3sup2sup"></a>**QoS de almacenamiento (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft. Health. FaultType. StorQos. InsufficientThroughput
* Gravedad: advertencia
* Motivo: *"el rendimiento de almacenamiento no es suficiente para satisfacer las reservas".*
* RecommendedAction: *"volver a configurar las directivas de QoS de almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft. Health. FaultType. StorQos. LostCommunication
* Gravedad: advertencia
* Motivo: *"el administrador de directivas QoS de almacenamiento perdió la comunicación con el volumen".*
* RecommendedAction: *"reinicie los nodos {Nodes}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft. Health. FaultType. StorQos. MisconfiguredFlow
* Gravedad: advertencia
* Motivo: *"uno o más consumidores de almacenamiento (normalmente virtual machines) están usando una directiva que no existe con el identificador {ID}."*
* RecommendedAction: *"volver a crear las directivas QoS de almacenamiento que faltan".*

<sup>1</sup> indica que el volumen ha alcanzado el 80% completo (gravedad secundaria) o 90% completo (gravedad principal).  
<sup>2</sup> indica que algunos. vhd del volumen no han cumplido su IOPS mínimo para más de un 10% (menor), 30% (principal) o 50% (crítico) de la ventana de 24 horas graduales.  

>[!NOTE]
> El mantenimiento de los componentes de contenedor de almacenamiento como ventiladores, fuentes de alimentación y sensores proviene de SCSI Enclosure Services (SES). Si el proveedor no proporciona esta información, el servicio de mantenimiento no puede mostrarla.  

## <a name="see-also"></a>Vea también

- [Servicio de mantenimiento de Windows Server 2016](health-service-overview.md)
