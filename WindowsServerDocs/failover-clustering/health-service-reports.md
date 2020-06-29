---
title: Servicio de mantenimiento informes
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: a1aedd4dc48abb38c33679f219a6825c6a9141bb
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473032"
---
# <a name="health-service-reports"></a>Servicio de mantenimiento informes

> Se aplica a: Windows Server 2019, Windows Server 2016

## <a name="what-are-reports"></a>¿Qué son los informes?

El Servicio de mantenimiento reduce el trabajo necesario para obtener información de rendimiento y capacidad en directo de su clúster de Espacios de almacenamiento directo. Un nuevo cmdlet proporciona una lista seleccionada de métricas esenciales que se recopilan de forma eficaz y se agregan dinámicamente entre nodos, con lógica integrada para detectar la pertenencia al clúster. Todos los valores son en tiempo real y únicamente en un momento dado.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use este cmdlet para obtener métricas de todo el clúster de Espacios de almacenamiento directo:

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

El parámetro de **recuento** opcional indica el número de conjuntos de valores que se van a devolver, en intervalos de un segundo.

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>
```

También puede obtener métricas para un volumen o servidor específico:

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

## <a name="usage-in-net-and-c"></a>Uso en .NET y C #

### <a name="connect"></a>Conectar

Para consultar el Servicio de mantenimiento, tendrá que establecer un **CimSession** con el clúster. Para ello, necesitará algunas cosas que solo están disponibles en .NET completo, lo que significa que no puede hacerlo directamente desde una aplicación web o móvil. Estos ejemplos de código usarán C \# , la opción más sencilla para esta capa de acceso a datos.

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

Antes de que pueda obtener errores o métricas, deberá obtener instancias de varios objetos pertinentes. En primer lugar, el ** \_ StorageSubSystem de msft** que representa espacios de almacenamiento directo en el clúster. Con esto, puede obtener todos los ** \_ StorageNode de msft** del clúster y todos los volúmenes **msft \_ **, los volúmenes de datos. Por último, también necesitará **el \_ StorageHealth de msft**, el propio servicio de mantenimiento.

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

Invoque **GetReport** para comenzar las muestras de streaming de una lista de métricas esenciales de experto en seleccionada, que se recopilan de forma eficaz y se agregan dinámicamente entre nodos, con lógica integrada para detectar la pertenencia al clúster. Los ejemplos se llegarán cada segundo después. Todos los valores son en tiempo real y únicamente en un momento dado.

Las métricas se pueden transmitir por secuencias para tres ámbitos: el clúster, cualquier nodo o cualquier volumen.

La lista completa de las métricas disponibles en cada ámbito en Windows Server 2016 se documenta a continuación.

### <a name="iobserveronnext"></a>IObserver. alnext ()

Este código de ejemplo usa el [modelo de diseño de observador](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx) para implementar un observador cuyo método **Next ()** se invocará cuando llegue cada nuevo ejemplo de métricas. Se llamará a su método **Alcompleted ()** si/cuando finalice el streaming. Por ejemplo, puede utilizarlo para reiniciar el streaming, por lo que continúa indefinidamente.

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

### <a name="begin-streaming"></a>Iniciar streaming

Una vez definido el observador, puede comenzar el streaming.

Especifique el **CimInstance** de destino al que desea que se alcancen las métricas. Puede ser el clúster, cualquier nodo o cualquier volumen.

El parámetro Count es el número de muestras antes de que finalice el streaming.

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

No es necesario decir que estas métricas se pueden visualizar, almacenar en una base de datos o usar de la forma que considere apropiada.

### <a name="properties-of-reports"></a>Propiedades de los informes

Cada ejemplo de métricas es un "informe" que contiene muchos "registros" correspondientes a las métricas individuales.

En el esquema completo, inspeccione las clases **msft \_ StorageHealthReport** y **msft \_ HealthRecord** en *storagewmi. mof*.

Cada métrica tiene solo tres propiedades, por esta tabla.

| **Propiedad** | **Ejemplo**       |
| -------------|-------------------|
| Nombre         | IOLatencyAverage  |
| Value        | 0,00021           |
| Unidades        | 3                 |

Unidades = {0, 1, 2, 3, 4}, donde 0 = "bytes", 1 = "BytesPerSecond", 2 = "CountPerSecond", 3 = "segundos" o 4 = "porcentaje".

## <a name="coverage"></a>Cobertura

A continuación se muestran las métricas disponibles para cada ámbito en Windows Server 2016.

### <a name="msft_storagesubsystem"></a>MSFT_StorageSubSystem

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


### <a name="msft_storagenode"></a>MSFT_StorageNode

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

### <a name="msft_volume"></a>MSFT_Volume

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

## <a name="additional-references"></a>Referencias adicionales

- [Servicio de mantenimiento de Windows Server 2016](health-service-overview.md)
