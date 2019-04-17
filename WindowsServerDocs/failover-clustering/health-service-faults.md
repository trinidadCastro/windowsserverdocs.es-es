---
title: Errores de servicio de estado
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: c9e1fb4568ee93739c49ccc1a13106b09161c5f3
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-faults"></a>Errores de servicio de estado
> Se aplica a Windows Server 2016

## <a name="what-are-faults"></a>¿Cuáles son los errores de página?

El servicio de estado constantemente supervisa el clúster directa de los espacios de almacenamiento para detectar problemas y generar "errores". Un nuevo cmdlet muestra los errores actuales, lo que permite comprobar el estado de la implementación sin mirar cada entidad fácilmente o característica a la vez. Errores de página están diseñadas para ser precisos, fácil de entender y que se pueda accionar.  

Cada error contiene cinco campos importantes:  

-   Gravedad
-   Descripción del problema
-   Pasos siguientes recomendada para solucionar el problema
-   Información de identificación de la entidad fallida
-   Su ubicación física (si procede)

Por ejemplo, este es un error grave típico:  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > La ubicación física se deriva de la configuración de dominio de errores. Para obtener más información sobre dominios de error, consulta [dominios de error en Windows Server 2016](fault-domains.md). Si no proporcionas esta información, el campo de ubicación será menos útil, por ejemplo, solo puede mostrar el número de ranura.  

## <a name="root-cause-analysis"></a>Análisis de la causa raíz

El servicio de estado puede evaluar la posible causalidad entre con errores entidades para identificar y combinar los errores que son consecuencia del mismo problema subyacente. RECONOCIENDO cadenas de efecto, esto hace que para notificar el menos banales. Por ejemplo, si un servidor está inactivo, se espera que las unidades en el servidor también será sin conexión. Por lo tanto, se presentará solo un error para la causa raíz - en este caso, el servidor.  

## <a name="usage-in-powershell"></a>Uso de PowerShell

Para ver los errores actuales en PowerShell, ejecuta este cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Esto devuelve los defectos que afectan el clúster directa de los espacios de almacenamiento de información general. A menudo, estos errores relacionados con hardware y la configuración. Si no hay ningún error, este cmdlet no devolverá nada.  

>[!NOTE]
> En un entorno de no producción y bajo su propio riesgo, puedes experimentar con esta característica por errores de activación tú mismo, por ejemplo, mediante la eliminación de un disco físico o cerrando un nodo. Una vez que ha aparecido el error, vuelva a insertar el disco físico o reiniciar que el nodo y el error desaparecerá de nuevo.

También puedes ver los errores que afectan a solo los volúmenes específicos o los recursos compartidos de archivos con los siguientes cmdlets:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Esto devuelve los errores que afectan a solo el volumen o el archivo de uso compartido concretas. A menudo, estos errores relacionados con planeamiento de capacidad, resistencia de datos o funciones como la calidad de servicio de almacenamiento o réplica de almacenamiento. 

## <a name="usage-in-net-and-c"></a>Uso de .NET y C#

### <a name="connect"></a>Conectar

Para consultar el servicio de salud, tendrás que establecer un **CimSession** con el clúster. Para ello, necesitarás algunas cosas que solo están disponibles en .NET completo, lo que significa que no podrá fácilmente hacerlo directamente desde un sitio web o una aplicación móvil. Estas muestras de código usará C\ #, la opción más sencilla para este nivel de acceso a datos.

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

El nombre de usuario debe ser un administrador local del equipo de destino.

Se recomienda que se construye la contraseña **SecureString** directamente desde la entrada del usuario en tiempo real, por lo tanto, su contraseña no se nunca almacena en la memoria de texto no cifrado. Esto ayuda a mitigar una variedad de problemas de seguridad. Pero en la práctica, es común para fines de creación de prototipos crearlo como se indicó anteriormente.

### <a name="discover-objects"></a>Descubrir objetos

Con la **CimSession** establecido, puedes consultar Instrumental de administración de Windows (WMI) en el clúster.

Para poder obtener errores o métricas, deberás obtener instancias de varios objetos pertinentes. Primero, la **MSFT\_StorageSubSystem** que representa directa de los espacios de almacenamiento en el clúster. Usarlo, puedes obtener cada **MSFT\_StorageNode** del clúster y cada **MSFT\_Volume**, los volúmenes de datos. Por último, necesitarás la **MSFT\_StorageHealth**, el estado de servicio, también.

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

Estos son los mismos objetos que obtienes en PowerShell mediante cmdlets como **Get StorageSubSystem**, **Get-nodos de almacenamientode información**, y **Get volumen**.

Puedes acceder a las mismas propiedades, documentadas en [clases de API de administración de almacenamiento](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx).

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

Invocar **diagnosticar** para obtener los errores actuales de ámbito en el destino **CimInstance**, que ser el clúster o cualquier otro volumen.

La lista completa de los errores de página disponibles en cada ámbito en Windows Server 2016 se describe más abajo.

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

### <a name="optional-myfault-class"></a>Opcional: MyFault clase

Puede que tenga sentido para TI construir y conservar la representación de errores de su propio. Por ejemplo, esto **MyFault** clase almacena varias propiedades de clave de errores, incluidos los **FaultId**, que pueden usarse más adelante para asociar la actualización o quitar las notificaciones, o para Deduplique en caso de que es el mismo error detecta varias veces, por cualquier motivo.

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

La lista completa de propiedades en cada error (**DiagnoseResult**) se detallan a continuación.

### <a name="fault-events"></a>Eventos de error

Cuando se crea, se quita o se actualizan errores, el servicio de estado genera eventos WMI. Estos son esenciales para mantener el estado de la aplicación sincronizados sin sondeo frecuente y pueden ayudar a cosas como determinar cuándo enviar alertas de correo electrónico, por ejemplo. Para suscribirse a estos eventos, este código de ejemplo usa el patrón de diseño del observador de nuevo.

En primer lugar, Suscríbete a **MSFT\_StorageFaultEvent** eventos.

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

A continuación, implementa un observador cuya **OnNext()** método se invocará cuando se genere un evento nuevo.

Cada evento contiene **ChangeType** que indica si un error que se crean, quitar o actualizado y los correspondientes **FaultId**.

Asimismo, contienen todas las propiedades de error.

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

### <a name="understand-fault-lifecycle"></a>Comprender el ciclo de vida de errores

Errores de página no están destinados a estar marcado como "visto" o resuelto el usuario. Se crean cuando el servicio de estado observa un problema y se eliminan automáticamente y solo cuando el servicio de estado ya no se puede observar el problema. En general, esto refleja que se ha corregido el problema.

Sin embargo, en algunos casos, errores de página pueden estar detectar otra vez el servicio de estado (por ejemplo, después de conmutación por error, o debido a la conectividad intermitente, etcetera.). Por este motivo, es posible tiene sentido mantener tu propia representación de errores de forma que puede Deduplique fácilmente. Esto es especialmente importante si envías alertas por correo electrónico o equivalente.

### <a name="properties-of-faults"></a>Propiedades de errores

Esta tabla presentan varias propiedades de clave del objeto de error. Para el esquema completo, inspeccionar la **MSFT\_StorageDiagnoseResult** clase *storagewmi.mof*.

| **Propiedad**              | **Ejemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Motivo                    | "El volumen está quedando sin espacio disponible".                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000S.N. 123456789                                  |
| FaultingObjectLocation    | Bastidor A06, RU 25, ranura 11                                        |
| RecommendedActions        | {"Expandir el volumen.", "Migrar las cargas de trabajo a otros volúmenes."}   |

**FaultId** único dentro del ámbito de un clúster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"Informativo", "Advertencia" y "Error"}, o equivalentes colores como el rojo, azul y amarillo.

**FaultingObjectDescription** parte de la información de hardware, normalmente en blanco para objetos de software.

**FaultingObjectLocation** información de ubicación para el hardware, normalmente en blanco para objetos de software.

**RecommendedActions** lista de las acciones recomendadas, que son independientes y sin ningún orden particular. Hoy en día, esta lista es a menudo de longitud 1.

## <a name="properties-of-fault-events"></a>Propiedades de eventos de error

Esta tabla presentan varias propiedades de clave del evento de error. Para el esquema completo, inspeccionar la **MSFT\_StorageFaultEvent** clase *storagewmi.mof*.

Ten en cuenta la **ChangeType**, que indica si se está creando un error grave, quitar o actualizado y la **FaultId**. Un evento también contiene todas las propiedades del error afectado.

| **Propiedad**              | **Ejemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Motivo                    | "El volumen está quedando sin espacio disponible".                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000S.N. 123456789                                  |
| FaultingObjectLocation    | Bastidor A06, RU 25, ranura 11                                        |
| RecommendedActions        | {"Expandir el volumen.", "Migrar las cargas de trabajo a otros volúmenes."}   |

**ChangeType** ChangeType = {0, 1, 2} = {"crear", "Quitar", "Actualizar"}.

## <a name="coverage"></a>Cobertura

En Windows Server 2016, el servicio de estado proporciona la cobertura de errores siguientes:  

### **<a name="physicaldisk-8"></a>Disco físico (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* Gravedad: advertencia
* Causa: *"el disco físico error".*
* RecommendedAction: *"Reemplazar el disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* Gravedad: advertencia
* Causa: *"Conectividad se ha perdido en el disco físico."*
* RecommendedAction: *"Comprobar que el disco físico está correctamente conectado y trabajando".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* Gravedad: advertencia
* Causa: *"el disco físico presenta periódico de falta de respuesta".*
* RecommendedAction: *"Reemplazar el disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* Gravedad: advertencia
* Causa: *"un error del disco físico se prevé que se producen pronto".*
* RecommendedAction: *"Reemplazar el disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* Gravedad: advertencia
* Causa: *"el disco físico se pone en cuarentena porque no es compatible con su proveedor de soluciones."*
* RecommendedAction: *"Reemplazar el disco físico con hardware compatible".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* Gravedad: advertencia
* Causa: *"el disco físico es en cuarentena porque su versión de firmware no es compatible con su proveedor de soluciones".*
* RecommendedAction: *"Actualizar el firmware en el disco físico a la versión de destino."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* Gravedad: advertencia
* Causa: *"el disco físico ha no se ha reconocido metadatos."*
* RecommendedAction: *"este disco puede contener datos de un grupo de almacenamiento desconocido. Primero asegúrate de que no haya ningún dato útil en este disco y después se restablece el disco."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* Gravedad: advertencia
* Causa: *"Error intentar actualizar el firmware en el disco físico".*
* RecommendedAction: *"Intente usar un binario de firmware diferente."*

### **<a name="virtual-disk-2"></a>Disco virtual (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* Gravedad: informativa
* Causa: *"algunos datos en este volumen no son totalmente flexibles. Permanece accesible."*
* RecommendedAction: *"Restaurando el estado de resistencia de los datos de".*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* Gravedad: crítica
* Causa: *"el volumen no es accesible. Algunos datos se pueden perder."*
* RecommendedAction: *"Compruebe la física o conectividad de todos los dispositivos de almacenamiento de red. Es podrán que debas restaurar a partir de copia de seguridad."*

### **<a name="pool-capacity-1"></a>Capacidad del grupo (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* Gravedad: advertencia
* Causa: *"grupo de almacenamiento no tiene la capacidad de reserva recomendada mínimo. Esto puede limitar su capacidad para restaurar el estado de resistencia de datos en caso de errores de la unidad."*
* RecommendedAction: *"Agregar capacidad adicional al grupo de almacenamiento o liberar capacidad. El mínimo recomendado reservar varía según la implementación, pero es el valor de la capacidad de las unidades de aproximadamente 2."*

### <a name="volume-capacity-2sup1sup"></a>**Capacidad de volumen (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravedad: advertencia
* Causa: *"el volumen está quedando sin espacio disponible".*
* RecommendedAction: *"Expandir el volumen o migrar las cargas de trabajo a otros volúmenes."*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravedad: crítica
* Causa: *"el volumen está quedando sin espacio disponible".*
* RecommendedAction: *"Expandir el volumen o migrar las cargas de trabajo a otros volúmenes."*

### **<a name="server-3"></a>Servidor (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft.Health.FaultType.Server.Down
* Gravedad: crítica
* Causa: *"no se puede alcanzar el servidor."*
* RecommendedAction: *"iniciar o reemplazar el servidor."*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft.Health.FaultType.Server.Isolated
* Gravedad: crítica
* Causa: *"el servidor está aislado del clúster debido a problemas de conectividad."*
* RecommendedAction: *"Si aislamiento persiste, comprueba las redes o migrar las cargas de trabajo a otros nodos."*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft.Health.FaultType.Server.Quarantined
* Gravedad: crítica
* Causa: *"el servidor se pone en cuarentena de clúster debido a errores periódicos."*
* RecommendedAction: *"Reemplazar el servidor o corregir la red".*

### **<a name="cluster-1"></a>Clúster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* Gravedad: crítica
* Causa: *"el clúster es un error de servidor fuera va disminuyendo."*
* RecommendedAction: *"comprueba el recurso testigo y reiniciar según sea necesario. Iniciar o reemplazar los servidores con errores".*

### **<a name="network-adapterinterface-4"></a>Adaptador de red o interfaz (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* Gravedad: advertencia
* Causa: *"la interfaz de red haya perdido la conexión."*
* RecommendedAction: *"Volver a conectar el cable de red".*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* Gravedad: advertencia
* Causa: *"{servidor} tiene falta conectados a la red de clúster {clúster} de adaptadores de red".*
* RecommendedAction: *"Conectar el servidor a la red de clúster falta".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* Gravedad: advertencia
* Causa: *"la interfaz de red ha tenido un error de hardware."*
* RecommendedAction: *"Reemplazar el adaptador de interfaz de red".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* Gravedad: advertencia
* Causa: *"la {interfaz de red} de la interfaz de red no está habilitada y no esté en uso".*
* RecommendedAction: *"Habilitar la interfaz de red."*

### **<a name="enclosure-6"></a>Gabinete (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* Gravedad: advertencia
* Causa: *"Comunicación se ha perdido al alojamiento de almacenamiento."*
* RecommendedAction: *"Inicio o reemplazar el alojamiento de almacenamiento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* Gravedad: advertencia
* Causa: *"en la posición {posición} del gabinete almacenamiento el abanico de error".*
* RecommendedAction: *"Reemplazar el ventilador en la caja del almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* Gravedad: advertencia
* Causa: *"el sensor actual en la posición {posición} del gabinete de almacenamiento de error".*
* RecommendedAction: *"Reemplazar un sensor actual en el alojamiento de almacenamiento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* Gravedad: advertencia
* Causa: *"el sensor de tensión en {posición} del gabinete de almacenamiento de error".*
* RecommendedAction: *"Reemplazar un sensor de tensión en la caja del almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* Gravedad: advertencia
* Causa: *"el controlador de IO en {posición} del gabinete almacenamiento ha fallado."*
* RecommendedAction: *"Reemplazar un controlador IO en la caja del almacenamiento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* Gravedad: advertencia
* Causa: *"el sensor de temperatura en {posición} del gabinete de almacenamiento de error".*
* RecommendedAction: *"Reemplazar un sensor de temperatura en la caja del almacenamiento".*

### **<a name="firmware-rollout-3"></a>Implementación de firmware (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* Gravedad: advertencia
* Causa: *"Actualmente no se puede realizar progreso mientras se realizan el firmware de implementación."*
* RecommendedAction: *"Comprueba todos los espacios de almacenamiento están en buenas condiciones y que no esté en modo de mantenimiento ningún dominio de error".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* Gravedad: advertencia
* Causa: *"implantación de Firmware se canceló debido a la información de la versión de firmware ilegibles o inesperado después de aplicar una actualización de firmware".*
* RecommendedAction: *"firmware de reinicio implantar una vez que se ha resuelto el problema de firmware."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* Gravedad: advertencia
* Causa: *"implantación de Firmware se canceló debido a los discos físicos demasiados errores un intento de actualización de firmware".*
* RecommendedAction: *"firmware de reinicio implantar una vez que se ha resuelto el problema de firmware."*

### <a name="storage-qos-3sup2sup"></a>**Almacenamiento de información QoS (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* Gravedad: advertencia
* Causa: *"rendimiento almacenamiento es insuficiente para satisfacer la reserva".*
* RecommendedAction: *"Volver a configurar las directivas de almacenamiento QoS".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* Gravedad: advertencia
* Causa: *"el Administrador de directivas de QoS de almacenamiento ha perdido la comunicación con el volumen".*
* RecommendedAction: *"Reinicie nodos {nodos de}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* Gravedad: advertencia
* Causa: *"uno o varios consumidores de almacenamiento (normalmente máquinas virtuales) están usando una directiva de no existente con el identificador {id}".*
* RecommendedAction: *"Volver a crear las directivas de almacenamiento QoS faltantes".*

<sup>1</sup> indica alcanzó el volumen completo del 80% (gravedad menor) o el 90% (gravedad principales).  
<sup>2</sup> indica algunos .vhd(s) en el volumen no cumplen sus IOPS mínimo para durante 10% (menor), 30% (principal) o 50% (crítico) de giro de la ventana de 24 horas.  

>[!NOTE]
> El estado de los componentes de alojamiento de almacenamiento como ventiladores, fuentes de alimentación y sensores se deriva de servicios de alojamiento de SCSI (SES). Si tu proveedor no proporciona esta información, el servicio de estado no puede mostrarlo.  

## <a name="see-also"></a>Consulta también

- [Servicio de estado en Windows Server 2016](health-service-overview.md)
