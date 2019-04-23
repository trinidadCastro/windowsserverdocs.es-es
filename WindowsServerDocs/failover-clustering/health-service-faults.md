---
title: Errores del servicio de mantenimiento
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 31a38eacea3af3c0a288d61a77a24b4fa45a1932
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843376"
---
# <a name="health-service-faults"></a>Errores del servicio de mantenimiento
> Se aplica a Windows Server 2016

## <a name="what-are-faults"></a>¿Cuáles son los errores

El servicio de mantenimiento supervisa constantemente el clúster de espacios de almacenamiento directo para detectar problemas y generar "errores". Un cmdlet nuevo muestra los errores actuales, lo que le permite comprobar el estado de la implementación sin mirar todas las entidades o características a su vez fácilmente. Los errores están diseñados para ser precisos, fáciles de entender y accionable.  

Cada error contiene cinco campos importantes:  

-   Severity
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
 > La ubicación física procede de la configuración del dominio de error. Para obtener más información acerca de los dominios de error, consulte [dominios de error de Windows Server 2016](fault-domains.md). Si no proporciona esta información, el campo de ubicación será menos útil, por ejemplo, puede mostrar solo el número de ranura.  

## <a name="root-cause-analysis"></a>Análisis de causa raíz

El servicio de mantenimiento puede evaluar el potencial de causalidad entre entidades para identificar y combinar los errores que son consecuencia del mismo problema subyacente produzca un error. Al reconocer las cadenas de efectos, habrá menos informes innecesarios. Por ejemplo, si un servidor está inactivo, se espera que las unidades en el servidor también estará sin conexión. Por lo tanto, sólo un error, se generará para la causa raíz: en este caso, el servidor.  

## <a name="usage-in-powershell"></a>Uso en PowerShell

Para ver los errores actuales en PowerShell, ejecute este cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Esto devuelve los errores que afectan el clúster de espacios de almacenamiento directo general. A menudo, estos errores están relacionados con hardware o la configuración. Si no hay ningún error, este cmdlet devolverá nada.  

>[!NOTE]
> En un entorno que no sea de producción y bajo su responsabilidad, puede experimentar con esta característica desencadenando errores, por ejemplo, al quitar un disco físico o apagar un nodo. Una vez que ha aparecido el error, vuelva a insertar el disco físico o el reinicio que del nodo y el error desaparecerá de nuevo.

También puede ver los errores que afectan solo a volúmenes específicos o recursos compartidos de archivos con los siguientes cmdlets:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Esto devuelve los errores que afectan a solo el volumen o archivo de recurso compartido específico. A menudo, estos errores se refieren a planear la capacidad, resistencia de datos o características como almacenamiento de calidad de servicio o réplica de almacenamiento. 

## <a name="usage-in-net-and-c"></a>Uso de .NET yC#

### <a name="connect"></a>Conectar

Para consultar el servicio de mantenimiento, deberá establecer un **CimSession** con el clúster. Para ello, necesitará algunas cosas que solo están disponibles en .NET completo, lo que significa que no puede fácilmente hacerlo directamente desde una aplicación web o móvil. Estos ejemplos de código usará C\#más sencillos choice para esta capa de acceso a datos.

``` 
...
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

Se recomienda que construir la contraseña **SecureString** directamente por el usuario en tiempo real, por lo que la contraseña nunca se almacena en memoria en texto no cifrado. Esto ayuda a mitigar una variedad de cuestiones de seguridad. Pero en la práctica, es habitual para fines de creación de prototipos construirlo anterior.

### <a name="discover-objects"></a>Detectar objetos

Con el **CimSession** establecida, puede consultar Windows Management Instrumentation (WMI) en el clúster.

Antes de obtener errores o las métricas, deberá obtener instancias de varios objetos relevantes. En primer lugar, el **MSFT\_StorageSubSystem** representa espacios de almacenamiento directo en el clúster. Con que, puede obtener cada **MSFT\_StorageNode** en el clúster y cada **MSFT\_volumen**, los volúmenes de datos. Por último, necesitará el **MSFT\_StorageHealth**, el servicio de mantenimiento, demasiado.

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

Estos son los mismos objetos que se obtendrá en PowerShell mediante los cmdlets como **Get-StorageSubSystem**, **Get-StorageNode**, y **Get-Volume**.

Se pueden obtener acceso a las mismas propiedades, documentadas en [clases de API de administración de almacenamiento](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx).

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

### <a name="query-faults"></a>Errores de consulta

Invocar **diagnosticar** para obtener los errores actuales con ámbito en el destino **CimInstance**, que ser de cualquier volumen o el clúster.

La lista completa de errores disponibles en cada ámbito de Windows Server 2016 se detalla a continuación.

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

### <a name="optional-myfault-class"></a>Opcional: Clase MyFault

Puede que tenga sentido para construir y conservar su propia representación de errores. Por ejemplo, esto **MyFault** clase almacena varias propiedades clave de errores, incluidos el **FaultId**, que puede usar más adelante para asociar la actualización o quitar notificaciones o desduplicar en caso de que se detectó el error mismo varias veces, por cualquier motivo.

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

La lista completa de las propiedades de cada error (**DiagnoseResult**) se describe más abajo.

### <a name="fault-events"></a>Eventos de error

Cuando se crean, se quitan o se actualizan errores, el servicio de mantenimiento genera eventos de WMI. Estos son esenciales para mantener sincronizados sin frecuente sondear el estado de la aplicación y pueden ayudarle con cosas como determinar cuándo se debe enviar alertas por correo electrónico, por ejemplo. Para suscribirse a estos eventos, este código de ejemplo usa el patrón de diseño de observador de nuevo.

En primer lugar, suscríbase a **MSFT\_StorageFaultEvent** eventos.

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

A continuación, implementar un observador cuyo **OnNext()** método se invocará cada vez que se genera un nuevo evento.

Cada evento contiene **ChangeType** que indica si un error se está creando, quitado o actualizado y la correspondiente **FaultId**.

Además, que contienen todas las propiedades de error.

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

### <a name="understand-fault-lifecycle"></a>Ciclo de vida de error

Errores no pretende ser marcadas como "vistas" o resueltos por el usuario. Se crean cuando el servicio de mantenimiento se observa un problema y se quitan automáticamente y solo cuando el servicio de mantenimiento ya no se puede observar el problema. En general, esto refleja que se ha corregido el problema.

Sin embargo, en algunos casos, es posible que detecten la errores por el servicio de mantenimiento (por ejemplo, tras la conmutación por error o debido a errores de conectividad intermitentes, etcetera.). Por este motivo, es posible que tiene sentido conservar su propia representación de errores, por lo que puede desduplicar fácilmente. Esto es especialmente importante si enviar alertas por correo electrónico o un grupo equivalente.

### <a name="properties-of-faults"></a>Propiedades de errores

Esta tabla presentan varias propiedades clave del objeto error. Para el esquema completo, inspeccionar la **MSFT\_StorageDiagnoseResult** clase *storagewmi.mof*.

| **Propiedad**              | **Ejemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Razón                    | "El volumen se está quedando sin espacio disponible."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Bastidor A06, RU 25, ranura 11                                        |
| RecommendedActions        | {"Expandir el volumen.", "Migrar las cargas de trabajo a otros volúmenes."}   |

**FaultId** Unique dentro del ámbito de un clúster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"Error", "Advertencia" y "Informational"}, o equivalente colores como azul, amarillo y rojo.

**FaultingObjectDescription** parte de la información de hardware, normalmente en blanco para los objetos de software.

**FaultingObjectLocation** información de ubicación para el hardware, por lo general en blanco para los objetos de software.

**RecommendedActions** lista de las acciones recomendadas, que son independientes y sin ningún orden determinado. En la actualidad, esta lista suele ser de longitud 1.

## <a name="properties-of-fault-events"></a>Propiedades de eventos de error

Esta tabla presentan varias propiedades clave del evento de error. Para el esquema completo, inspeccionar la **MSFT\_StorageFaultEvent** clase *storagewmi.mof*.

Tenga en cuenta la **ChangeType**, lo que indica si se está creando un error, quitado o actualizado y el **FaultId**. Un evento también contiene todas las propiedades del error afectado.

| **Propiedad**              | **Ejemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Razón                    | "El volumen se está quedando sin espacio disponible."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Bastidor A06, RU 25, ranura 11                                        |
| RecommendedActions        | {"Expandir el volumen.", "Migrar las cargas de trabajo a otros volúmenes."}   |

**ChangeType** ChangeType = { 0, 1, 2 } = { "Create", "Remove", "Update" }.

## <a name="coverage"></a>Cobertura

En Windows Server 2016, el servicio de mantenimiento ofrece la cobertura del error siguiente:  

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* Gravedad: Advertencia
* Motivo: *"El disco físico error".*
* RecommendedAction: *"Reemplazar el disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* Gravedad: Advertencia
* Motivo: *"Se perdió la conectividad en el disco físico."*
* RecommendedAction: *"Compruebe que el disco físico está correctamente conectado y no laborables".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* Gravedad: Advertencia
* Motivo: *"El disco físico presenta periódico de falta de respuesta".*
* RecommendedAction: *"Reemplazar el disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* Gravedad: Advertencia
* Motivo: *"Un error del disco físico se prevé que se producen en breve."*
* RecommendedAction: *"Reemplazar el disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* Gravedad: Advertencia
* Motivo: *"El disco físico se puso en cuarentena porque no es compatible con su proveedor de soluciones."*
* RecommendedAction: *"Reemplazar el disco físico por hardware compatible."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* Gravedad: Advertencia
* Motivo: *"El disco físico está en cuarentena porque su versión de firmware no es compatible con su proveedor de soluciones."*
* RecommendedAction: *"Actualizar el firmware del disco físico a la versión de destino".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* Gravedad: Advertencia
* Motivo: *"El disco físico tiene metadatos no reconocido".*
* RecommendedAction: *"Este disco puede contener datos de un grupo de almacenamiento desconocido. En primer lugar asegúrese de que no hay ningún dato útil en este disco y después se restablece el disco."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* Gravedad: Advertencia
* Motivo: *"Error al intentar actualizar el firmware del disco físico."*
* RecommendedAction: *"Pruebe a usar un binario de firmware diferente".*

### <a name="virtual-disk-2"></a>**Disco virtual (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* Gravedad: Informativo
* Motivo: *"Algunos datos de este volumen no están totalmente resistentes. Sigue siendo accesible."*
* RecommendedAction: *"Restauración de la resistencia de los datos".*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* Gravedad: Crítico
* Motivo: *"El volumen es inaccesible. Es posible que se pierdan algunos datos."*
* RecommendedAction: *"Compruebe físico o conectividad de todos los dispositivos de almacenamiento de red. Es posible que necesita restaurar desde copia de seguridad."*

### <a name="pool-capacity-1"></a>**Capacidad del grupo (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* Gravedad: Advertencia
* Motivo: *"El bloque de almacenamiento no tiene la capacidad de reserva recomendada mínima. Esto puede limitar su capacidad de restaurar la resistencia de datos en el caso de errores de la unidad."*
* RecommendedAction: *"Agregar capacidad adicional al bloque de almacenamiento o liberar capacidad. El mínimo recomendado reserva varía según la implementación, pero es natural de aproximadamente 2 de las unidades de capacidad."*

### <a name="volume-capacity-2sup1sup"></a>**Capacidad del volumen (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravedad: Advertencia
* Motivo: *"El volumen se está quedando sin espacio disponible."*
* RecommendedAction: *"Expandir el volumen o migrar las cargas de trabajo a otros volúmenes."*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravedad: Crítico
* Motivo: *"El volumen se está quedando sin espacio disponible."*
* RecommendedAction: *"Expandir el volumen o migrar las cargas de trabajo a otros volúmenes."*

### <a name="server-3"></a>**Servidor (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft.Health.FaultType.Server.Down
* Gravedad: Crítico
* Motivo: *"No se puede tener acceso al servidor."*
* RecommendedAction: *"Empezar o reemplazar el servidor".*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft.Health.FaultType.Server.Isolated
* Gravedad: Crítico
* Motivo: *"El servidor está aislado del clúster debido a problemas de conectividad."*
* RecommendedAction: *"Si persiste el aislamiento, compruebe las redes o migrar las cargas de trabajo a otros nodos".*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft.Health.FaultType.Server.Quarantined
* Gravedad: Crítico
* Motivo: *"El servidor está en cuarentena por el clúster debido a errores periódicos."*
* RecommendedAction: *"Reemplazar el servidor o reparar la red".*

### <a name="cluster-1"></a>**Clúster de (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* Gravedad: Crítico
* Motivo: *"El clúster es un error de servidor fuera deja de funcionar".*
* RecommendedAction: *"Compruebe el recurso de testigo y reinicie según sea necesario. Iniciar o reemplazar servidores con errores".*

### <a name="network-adapterinterface-4"></a>**Adaptador de red o interfaz (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* Gravedad: Advertencia
* Motivo: *"La interfaz de red ha se han desconectado".*
* RecommendedAction: *"Vuelva a conectar el cable de red".*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* Gravedad: Advertencia
* Motivo: *"El servidor {server} tiene falta conectado a la red de clústeres {red en clúster} de adaptadores de red".*
* RecommendedAction: *"Conectar el servidor a la red de clústeres que faltan".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* Gravedad: Advertencia
* Motivo: *"La interfaz de red ha tenido un error de hardware".*
* RecommendedAction: *"Reemplazar el adaptador de interfaz de red".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* Gravedad: Advertencia
* Motivo: *"La interfaz de red {interfaz de red} no está habilitada y no se utiliza".*
* RecommendedAction: *"Habilitar la interfaz de red".*

### <a name="enclosure-6"></a>**Alojamiento (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* Gravedad: Advertencia
* Motivo: *"Comunicación se ha perdido en el contenedor de almacenamiento".*
* RecommendedAction: *"Empezar o reemplazar el contenedor de almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* Gravedad: Advertencia
* Motivo: *"El ventilador en posición {posición} del gabinete de almacenamiento de información de error".*
* RecommendedAction: *"Reemplazar el ventilador del contenedor de almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* Gravedad: Advertencia
* Motivo: *"El sensor de corriente en posición {posición} del alojamiento almacenamiento error".*
* RecommendedAction: *"Reemplazar un sensor de corriente en el contenedor de almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* Gravedad: Advertencia
* Motivo: *"El detector de voltaje en posición {posición} del alojamiento almacenamiento error".*
* RecommendedAction: *"Reemplazar un sensor de voltaje del contenedor de almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* Gravedad: Advertencia
* Motivo: *"El controlador de E/S en la posición {posición} del contenedor de almacenamiento ha fallado."*
* RecommendedAction: *"Reemplazar un controlador de E/S en el contenedor de almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* Gravedad: Advertencia
* Motivo: *"El sensor de temperatura en posición {posición} del gabinete de almacenamiento de información de error".*
* RecommendedAction: *"Reemplazar un sensor de temperatura en el contenedor de almacenamiento".*

### <a name="firmware-rollout-3"></a>**Implementación de firmware (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* Gravedad: Advertencia
* Motivo: *"Actualmente no se puede avanzar al realizar la implementación de firmware."*
* RecommendedAction: *"Compruebe todos los espacios de almacenamiento están en buen Estados y que ningún dominio de error está actualmente en modo de mantenimiento".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* Gravedad: Advertencia
* Motivo: *"Implementación de firmware se canceló debido a la información de versión de firmware no se puede leer o inesperado después de aplicar una actualización de firmware".*
* RecommendedAction: *"Reinicio firmware desplegar una vez que se ha resuelto el problema de firmware."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* Gravedad: Advertencia
* Motivo: *"Implementación de firmware se canceló debido a demasiados discos físicos que se producen errores en un intento de actualización de firmware".*
* RecommendedAction: *"Reinicio firmware desplegar una vez que se ha resuelto el problema de firmware."*

### <a name="storage-qos-3sup2sup"></a>**QoS de almacenamiento (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* Gravedad: Advertencia
* Motivo: *"Es insuficiente para satisfacer las reservas de rendimiento de almacenamiento".*
* RecommendedAction: *"Vuelva a configurar las directivas de QoS de almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* Gravedad: Advertencia
* Motivo: *"El Administrador de directivas de QoS de almacenamiento ha perdido la comunicación con el volumen".*
* RecommendedAction: *"Reinicie los nodos {en los nodos de}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* Gravedad: Advertencia
* Motivo: *"Uno o varios clientes de almacenamiento (normalmente máquinas virtuales) están usando una directiva de inexistente con identificador {id}".*
* RecommendedAction: *"Vuelva a crear las directivas de QoS de almacenamiento que faltan."*

<sup>1</sup> indica que ha alcanzado el volumen total de 80% (gravedad menor) o el 90% (gravedad principal).  
<sup>2</sup> indica que algunos archivos .vhd en el volumen no cumplen sus IOPS mínimos sobre 10% (menor), 30% (principal) o el 50% (crítico) de revertir la ventana de 24 horas.  

>[!NOTE]
> El mantenimiento de los componentes de contenedor de almacenamiento como ventiladores, fuentes de alimentación y sensores proviene de SCSI Enclosure Services (SES). Si el proveedor no proporciona esta información, el servicio de mantenimiento no puede mostrarla.  

## <a name="see-also"></a>Vea también

- [Servicio de mantenimiento de Windows Server 2016](health-service-overview.md)
