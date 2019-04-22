---
title: Informes de servicio de mantenimiento
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: bc21b9fdec5700fec23dc6af7ca15873ded34bea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821966"
---
# <a name="health-service-reports"></a>Informes de servicio de mantenimiento
> Se aplica a Windows Server 2016

## <a name="what-are-reports"></a>¿Cuáles son los informes  

El servicio de mantenimiento reduce el trabajo necesario para obtener información de capacidad y rendimiento en vivo desde el clúster de espacios de almacenamiento directo. Un cmdlet nuevo proporciona una lista exclusiva de las métricas esenciales, que se recopilan de forma eficaz y se agregan dinámicamente por los nodos, con lógica integrada para detectar la pertenencia al clúster. Todos los valores son en tiempo real y únicamente en un momento dado.  

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use este cmdlet para obtener las métricas para todo el clúster de espacios de almacenamiento directo:

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

El elemento opcional **recuento** parámetro indica cuántos conjuntos de valores se devuelven, en intervalos de un segundo.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

También puede obtener las métricas para un volumen específico o servidor:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

Invocar **GetReport** para iniciar el streaming de ejemplos de una lista seleccionada de experto de las métricas esenciales, que se recopilan de forma eficaz y se agregan dinámicamente por los nodos, con lógica integrada para detectar la pertenencia al clúster. Ejemplos de llegará a partir de entonces cada segundo. Todos los valores son en tiempo real y únicamente en un momento dado.

Las métricas se pueden transmitir para tres ámbitos: cualquier volumen, cualquier nodo o el clúster.

La lista completa de métricas disponibles en cada ámbito de Windows Server 2016 se detalla a continuación.

### <a name="iobserveronnext"></a>IObserver.OnNext()

Este código de ejemplo usa el [patrón de diseño de observador](https://msdn.microsoft.com/en-us/library/ee850490(v=vs.110).aspx) para implementar un observador cuyo **OnNext()** método se invocará cuando llega a cada nuevo ejemplo de métricas. Su **OnCompleted()** se llamará al método si/cuando los extremos de streaming. Por ejemplo, puede usarlo para reiniciar la transmisión por secuencias, por lo que continúa indefinidamente.

```
class MetricsObserver<T> : IObserver<T>
{
    public void OnNext(T Result)
    {
        // Cast
        CimMethodStreamedResult StreamedResult = Result as CimMethodStreamedResult;

        if (StreamedResult != null)
        {
            // For illustration, you could store the metrics in this dictionary
            Dictionary<string, string> Metrics = new Dictionary<string, string>();

            // Unpack
            CimInstance Report = (CimInstance)StreamedResult.ItemValue;
            IEnumerable<CimInstance> Records = (IEnumerable<CimInstance>)Report.CimInstanceProperties["Records"].Value;
            foreach (CimInstance Record in Records)
            {
                /// Each Record has "Name", "Value", and "Units"
                Metrics.Add(
                    Record.CimInstanceProperties["Name"].Value.ToString(),
                    Record.CimInstanceProperties["Value"].Value.ToString()
                    );
            }

            // TODO: Whatever you want!
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Reinvoke BeginStreamingMetrics(), defined in the next section
    }
}
```

### <a name="begin-streaming"></a>Iniciar el streaming

Con el observador definido, puede empezar a transmitir.

Especifique el destino **CimInstance** al que se desean que las métricas de ámbito. Puede ser cualquier volumen, cualquier nodo o el clúster.

El parámetro count es el número de muestras antes de extremos de streaming.

```
CimInstance Target = Cluster; // From among the objects discovered in DiscoverObjects()

public void BeginStreamingMetrics(CimSession Session, CimInstance HealthService, CimInstance Target)
{
    // Set Parameters
    CimMethodParametersCollection MetricsParams = new CimMethodParametersCollection();
    MetricsParams.Add(CimMethodParameter.Create("TargetObject", Target, CimType.Instance, CimFlags.In));
    MetricsParams.Add(CimMethodParameter.Create("Count", 999, CimType.UInt32, CimFlags.In));
    // Enable WMI Streaming
    CimOperationOptions Options = new CimOperationOptions();
    Options.EnableMethodResultStreaming = true;
    // Invoke API
    CimAsyncMultipleResults<CimMethodResultBase> InvokeHandler;
    InvokeHandler = Session.InvokeMethodAsync(
        HealthService.CimSystemProperties.Namespace, HealthService, "GetReport", MetricsParams, Options
        );
    // Subscribe the Observer
    MetricsObserver<CimMethodResultBase> Observer = new MetricsObserver<CimMethodResultBase>(this);
    IDisposable Disposeable = InvokeHandler.Subscribe(Observer);
}
```

Obviamente, se pueden visualizar estas métricas, almacenado en una base de datos o usado en la forma que estime oportuno.

### <a name="properties-of-reports"></a>Propiedades de informes

Cada ejemplo de métricas es un "informe" que contiene muchos "registros" correspondiente a las mediciones individuales.

Para el esquema completo, inspeccionar la **MSFT\_StorageHealthReport** y **MSFT\_HealthRecord** las clases de *storagewmi.mof*.

Cada métrica tiene solo tres propiedades, según esta tabla.

| **Propiedad** | **Ejemplo**       |
| -------------|-------------------|
| Nombre         | IOLatencyAverage  |
| Valor        | 0.00021           |
| Unidades        | 3                 |

Unidades = {0, 1, 2, 3, 4}, donde 0 = "Bytes", 1 = "bytespersecond" o, 2 = "CountPerSecond", 3 = "Segundos", o bien, 4 = "Porcentaje".

## <a name="coverage"></a>Cobertura

A continuación son las métricas disponibles para cada ámbito de Windows Server 2016.

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

| **Name**                        | **Unidades** |
|---------------------------------|-----------|
| CPUUsage                        | 4         |
| CapacityPhysicalPooledAvailable | 0         |
| CapacityPhysicalPooledTotal     | 0         |
| CapacityPhysicalTotal           | 0         |
| CapacityPhysicalUnpooled        | 0         |
| CapacityVolumesAvailable        | 0         |
| CapacityVolumesTotal            | 0         |
| IOLatencyAverage                | 3         |
| IOLatencyRead                   | 3         |
| IOLatencyWrite                  | 3         |
| IOPSRead                        | 2         |
| IOPSTotal                       | 2         |
| IOPSWrite                       | 2         |
| IOThroughputRead                | 1         |
| IOThroughputTotal               | 1         |
| IOThroughputWrite               | 1         |
| MemoryAvailable                 | 0         |
| MemoryTotal                     | 0         |


### <a name="msftstoragenode"></a>MSFT_StorageNode

| **Name**            | **Unidades** |
|---------------------|-----------|
| CPUUsage            | 4         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |
| MemoryAvailable     | 0         |
| MemoryTotal         | 0         |

### <a name="msftvolume"></a>MSFT_Volume

| **Name**            | **Unidades** |
|---------------------|-----------|
| CapacityAvailable   | 0         |
| CapacityTotal       | 0         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |

## <a name="see-also"></a>Vea también

- [Servicio de mantenimiento de Windows Server 2016](health-service-overview.md)
