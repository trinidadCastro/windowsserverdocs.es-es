---
title: Informes de estado de servicio
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: bc21b9fdec5700fec23dc6af7ca15873ded34bea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-reports"></a>Informes de estado de servicio
> Se aplica a Windows Server 2016

## <a name="what-are-reports"></a>¿Cuáles son los informes?  

El servicio de estado reduce el trabajo necesario para obtener rendimiento dinámico y la información sobre la capacidad del clúster directa de los espacios de almacenamiento. Un nuevo cmdlet proporciona una lista protegida de métricas esenciales, que se recopila de forma eficaz y agregadas dinámicamente en nodos, con lógica integrada para detectar la pertenencia al clúster. Todos los valores son en tiempo real y en un momento solo.  

## <a name="usage-in-powershell"></a>Uso de PowerShell

Usa este cmdlet para obtener métricas de todo el clúster de espacios de almacenamiento directa:

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

Opcional **recuento** parámetro indica cuántas conjuntos de valores que devolver, en intervalos de un segundo.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

También puedes obtener métricas para un volumen específico o un servidor:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

Invocar **GetReport** para iniciar la transmisión por secuencias de muestras de una lista supervisada experto de métricas esenciales, que se recopila de forma eficaz y agregadas dinámicamente en nodos, con lógica integrada para detectar la pertenencia al clúster. Muestras llegará a cada segundo posteriormente. Todos los valores son en tiempo real y en un momento solo.

Se pueda transmitir métricas para tres ámbitos: del clúster, cualquier nodo o cualquier otro volumen.

La lista completa de las métricas disponibles en cada ámbito en Windows Server 2016 se describe más abajo.

### <a name="iobserveronnext"></a>IObserver.OnNext()

Este código de muestra usa el [patrón de diseño del observador](https://msdn.microsoft.com/en-us/library/ee850490(v=vs.110).aspx) para implementar un observador cuya **OnNext()** método se invocará cuando llega a cada nueva muestra de métricas. Su **OnCompleted()** método se llamará si cuando hagas streaming de extremos. Por ejemplo, es posible que usas para reiniciar la transmisión por secuencias, por lo que continúa indefinidamente.

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

### <a name="begin-streaming"></a>Empieza con el streaming

Con el observador definido, puedes empezar a transmisión por secuencias.

Especificar el destino **CimInstance** al que se desea las métricas de ámbito. Puede ser el clúster, cualquier nodo o cualquier otro volumen.

El parámetro count es el número de muestras antes de transmisión por secuencias finaliza.

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

Obviamente, estas métricas se pueden visualizar en, almacenada en una base de datos o usados en cualquier modo que consideres más oportuno.

### <a name="properties-of-reports"></a>Propiedades de informes

Cada muestra de métricas es un "informe" que contiene muchos "registros" correspondiente a las mediciones individuales.

Para el esquema completo, inspeccionar la **MSFT\_StorageHealthReport** y **MSFT\_HealthRecord** las clases *storagewmi.mof*.

Cada métrica tiene solo tres propiedades, por esta tabla.

| **Propiedad** | **Ejemplo**       |
| -------------|-------------------|
| Nombre         | IOLatencyAverage  |
| Valor        | 0.00021           |
| Unidades        | 3                 |

Unidades = {0, 1, 2, 3, 4}, donde 0 = "Bytes", 1 = "BytesPerSecond", 2 = "CountPerSecond", 3 = "Segundos" o 4 = "Porcentaje".

## <a name="coverage"></a>Cobertura

A continuación están disponibles para cada ámbito en Windows Server 2016 las métricas.

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

| **Nombre**                        | **Unidades** |
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

| **Nombre**            | **Unidades** |
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

| **Nombre**            | **Unidades** |
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

## <a name="see-also"></a>Consulta también

- [Servicio de estado en Windows Server 2016](health-service-overview.md)
