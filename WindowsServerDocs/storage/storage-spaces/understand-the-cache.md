---
title: Descripción de la memoria caché de Espacios de almacenamiento directo
ms.assetid: 69b1adc0-ee64-4eed-9732-0fb216777992
ms.author: cosdar
manager: dongill
ms.topic: article
author: cosmosdarwin
ms.date: 09/21/2020
ms.localizationpriority: medium
ms.openlocfilehash: 502b04676fcb9a9c7342e701e71be473890f9668
ms.sourcegitcommit: 8a826e992f28a70e75137f876a5d5e61238a24e4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365358"
---
# <a name="understanding-the-cache-in-storage-spaces-direct"></a>Descripción de la memoria caché de Espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

[Espacios de almacenamiento directo](storage-spaces-direct-overview.md) incluye una memoria caché incorporada en el lado servidor para maximizar el rendimiento del almacenamiento. Se trata de una memoria caché grande, persistente y de lectura *y* escritura en tiempo real. La caché se configura automáticamente cuando se habilita Espacios de almacenamiento directo. En la mayoría de los casos, no se requiere ninguna administración manual.
El funcionamiento de la caché depende de los tipos de unidades presentes.

El vídeo siguiente incluye detalles sobre cómo funciona el almacenamiento en caché de Espacios de almacenamiento directo, así como otras consideraciones de diseño.

<strong>Consideraciones de diseño de Espacios de almacenamiento directo</strong><br>(20 minutos)<br>
<iframe src="https://channel9.msdn.com/Blogs/windowsserver/Design-Considerations-for-Storage-Spaces-Direct/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="drive-types-and-deployment-options"></a>Tipos de unidad y opciones de implementación

Actualmente, Espacios de almacenamiento directo funciona con cuatro tipos de dispositivos de almacenamiento:

| Tipo de unidad | Descripción |
|----------------------|--------------------------|
|![PMem](media/understand-the-cache/pmem-100px.png)|**PMem** hace referencia a la memoria persistente, un nuevo tipo de almacenamiento de baja latencia y alto rendimiento.|
|![NVMe](media/understand-the-cache/NVMe-100px.png)|**NVMe** (memoria no volátil rápida) hace referencia a las unidades de estado sólido que se colocan directamente en el bus PCIe. Los factores de forma comunes son 2,5" U.2, AIC (Add-In Card) PCIe y M.2. NVMe ofrece mayor rendimiento de IOPS y E/S con latencia más baja que ningún otro tipo de unidad que se admita hoy en día excepto PMem.|
|![SSD](media/understand-the-cache/SSD-100px.png)|**SSD** hace referencia a las unidades de estado sólido, que se conectan a través de un dispositivo SATA o SAS convencional.|
|![HDD](media/understand-the-cache/HDD-100px.png)|**HDD** hace referencia a unidades de disco duro magnéticas de rotación que ofrecen gran capacidad de almacenamiento.|

Estos se pueden combinar de varias maneras que se agrupan en dos categorías: "todo flash" e "híbrida".

### <a name="all-flash-deployment-possibilities"></a>Posibilidades de implementación todo flash

Todas las implementaciones all-flash tienen el objetivo de maximizar el rendimiento del almacenamiento y no incluyen unidades de disco duro (HDD) rotacionales.

![Posibilidades de implementación de todo flash](media/understand-the-cache/All-Flash-Deployment-Possibilities.png)

### <a name="hybrid-deployment-possibilities"></a>Posibilidades de implementación híbrida

Las implementaciones híbridas tienen como objetivo equilibrar el rendimiento y la capacidad o maximizar la capacidad, e incluyen unidades de disco duro (HDD) rotacionales.

![Posibilidades de implementación híbridas](media/understand-the-cache/Hybrid-Deployment-Possibilities.png)

## <a name="cache-drives-are-selected-automatically"></a>Las unidades de caché se seleccionan de forma automática.

En las implementaciones con varios tipos de unidades, Espacios de almacenamiento directo usa automáticamente todas las unidades del tipo "más rápido" para el almacenamiento en caché. Las unidades restantes se usan para la capacidad.

El tipo "más rápido" se determina según la siguiente jerarquía.

![Jerarquía de tipos de unidad](media/understand-the-cache/Drive-Type-Hierarchy.png)

Por ejemplo, si tiene NVMe y SSD, la NVMe se almacenará en caché para los SSD.

Si tiene SSD y HDD, los SSD se almacenarán en caché para los discos duros.

   >[!NOTE]
   > Las unidades de caché no contribuyen a la capacidad de almacenamiento utilizable. Todos los datos almacenados en la caché también se almacenan en otro lugar, o bien lo harán una vez que se retiren del almacenamiento provisional. Esto significa que la capacidad total de almacenamiento sin procesar de la implementación es la suma de las unidades de capacidad únicamente.

Si todas las unidades son del mismo tipo, no se configura automáticamente ninguna caché. Tienes posibilidad de configurar manualmente unidades de mayor resistencia para caché en el caso de unidades de menor resistencia del mismo tipo. Consulta la sección [Configuración manual](#manual-configuration) para aprender a hacerlo.

   >[!TIP]
   > En las implementaciones con todo NVMe o todo SSD, especialmente en escalados muy pequeños, no tener unidades "dedicadas" en la memoria caché puede mejorar la eficacia del almacenamiento de forma significativa.

## <a name="cache-behavior-is-set-automatically"></a>El comportamiento de la memoria caché se establece automáticamente

El comportamiento de la memoria caché se determina automáticamente según los tipos de las unidades de las que se realiza el almacenamiento en caché. Al realizar el almacenamiento en caché de las unidades de estado sólido (por ejemplo, almacenamiento en caché NVMe para SSD), solo las escrituras se almacenan en caché. Al almacenar en caché las unidades de disco duro (por ejemplo, almacenar en caché los discos SSD para las unidades HDD), tanto las lecturas como las escrituras se almacenan en caché.

![Comportamiento de lectura y escritura en caché](media/understand-the-cache/Cache-Read-Write-Behavior.png)

### <a name="write-only-caching-for-all-flash-deployments"></a>Almacenamiento en caché de solo escritura para las implementaciones todo flash

Al realizar el almacenamiento en caché de las unidades de estado sólido (NVMe o SSD), solo las escrituras se almacenan en caché. Esto reduce el desgaste de las unidades de capacidad porque muchas escrituras y reescrituras pueden fusionarse en la memoria caché y luego se mueven según sea necesario, lo que reduce el tráfico acumulativo hacia las unidades de capacidad y prolonga su duración. Por esta razón, se recomienda seleccionar unidades de [mayor resistencia y optimizadas para escritura](http://whatis.techtarget.com/definition/DWPD-device-drive-writes-per-day) para la caché. Las unidades de capacidad pueden tener, por consiguiente, una menor resistencia de escritura.

Dado que las lecturas no afectan significativamente a la vida útil de la memoria flash, y que las unidades de estado sólido universalmente ofrecen baja latencia de lectura, las lecturas no se almacenan en caché, sino que se proporcionan directamente desde las unidades de capacidad (excepto cuando los datos se escribieron tan recientemente que aún no se han movido). Esto permite que la memoria caché se dedique por completo a las escrituras, lo cual aumenta al máximo su eficacia.

Como resultado, las características de escritura, como la latencia de escritura, están determinadas por las unidades de caché, mientras que las características de lectura están determinadas por las unidades de capacidad. Ambos son coherentes, predecibles y uniformes.

### <a name="readwrite-caching-for-hybrid-deployments"></a>Almacenamiento en caché de lectura y escritura para implementaciones híbridas

Al almacenar en caché las unidades de disco duro (HDD), las lecturas *y* las escrituras se almacenan en caché para proporcionar una latencia similar a la de la memoria flash (a menudo 10 veces mejor) para ambas. La memoria caché de lectura almacena datos que se han leído recientemente y con frecuencia para obtener un acceso rápido y para minimizar el tráfico aleatorio a las unidades de disco duro. (Debido a los retrasos por búsqueda y rotación, la latencia y la pérdida de tiempo que provoca el acceso aleatorio a una unidad de disco duro es importante). Las escrituras se almacenan en caché para absorber las ráfagas y, al igual que antes, para fusionar las escrituras y las reescrituras y minimizar el tráfico acumulativo hacia las unidades de capacidad.

Espacios de almacenamiento directo implementa un algoritmo que elimina la aleatoriedad de las escrituras antes de moverlas, para emular un modelo de E/S en disco que parezca secuencial aunque la E/S real proveniente de la carga de trabajo (por ejemplo, máquinas virtuales) sea aleatoria. Esto maximiza el número de operaciones de entrada y salida y el rendimiento de las unidades HDD.

### <a name="caching-in-deployments-with-drives-of-all-three-types"></a>Almacenamiento en caché en implementaciones con unidades de los tres tipos

Cuando hay unidades de los tres tipos, las unidades de NVMe proporcionan almacenamiento en caché para los discos SSD y HDD. El comportamiento es como el descrito anteriormente: solo se almacenan en caché las escrituras para los discos SSD, y las lecturas y escrituras para las unidades de disco duro. La carga del almacenamiento en caché para las unidades de disco duro se distribuye uniformemente entre las unidades de caché.

## <a name="summary"></a>Resumen

En la siguiente tabla se resumen qué unidades se usan para almacenar en caché, cuáles se usan para capacidad y cuál es el comportamiento del almacenamiento en caché para cada posibilidad de implementación.

| Implementación     | Unidades de caché                        | Unidades de capacidad | Comportamiento de la caché (predeterminado)  |
| -------------- | ----------------------------------- | --------------- | ------------------------- |
| Todo NVMe         | Ninguno (Opcional: configurar manualmente) | NVMe            | Solo escritura (si está configurado)  |
| Todo SSD          | Ninguno (Opcional: configurar manualmente) | SSD             | Solo escritura (si está configurado)  |
| NVMe + SSD       | NVMe                                | SSD             | Solo escritura                  |
| NVMe + HDD       | NVMe                                | HDD             | Lectura + escritura                |
| SSD + HDD        | SSD                                 | HDD             | Lectura + escritura                |
| NVMe + SSD + HDD | NVMe                                | SSD + HDD       | Lectura + escritura para HDD, solo escritura para SSD  |

## <a name="server-side-architecture"></a>Arquitectura en el lado servidor

La memoria caché se implementa en el nivel de unidad: las unidades de caché individuales dentro de un servidor están enlazadas a una o varias unidades de capacidad en el mismo servidor.

Dado que la memoria caché está debajo del resto de la pila de almacenamiento definido por software de Windows, no tiene ni necesita el reconocimiento de conceptos como espacios de almacenamiento o tolerancia a errores. Puede considerarlo como la creación de unidades "híbridas" (en parte flash, en parte disco) que luego se presentan a Windows. Al igual que con una unidad híbrida real, el movimiento en tiempo real de los datos fríos y calientes entre las partes más rápidas y más lentas de los medios físicos es casi invisible en el exterior.

Dado que la resistencia de la característica Espacios de almacenamiento directo ocurre, como mínimo, en el nivel de servidor (es decir, las copias de datos siempre se escriben en servidores diferentes, a lo sumo una copia por servidor), los datos que están en la memoria caché aprovechan la misma resistencia que los datos que no están en la caché.

![Arquitectura de la caché en el lado servidor](media/understand-the-cache/Cache-Server-Side-Architecture.png)

Por ejemplo, cuando se usa la creación de reflejo triple, se escriben tres copias de los datos en servidores diferentes, y se colocan en la memoria caché. Independientemente de si se retiran del almacenamiento provisional posteriormente o no, siempre existirán tres copias.

## <a name="drive-bindings-are-dynamic"></a>Los enlaces de unidades son dinámicos

El enlace entre las unidades de caché y de capacidad puede tener cualquier proporción, desde 1:1 hasta 1:12 e incluso más. Se ajusta dinámicamente cada vez que se agregan o quitan unidades como, por ejemplo, al escalar verticalmente o después de errores. Esto significa que puede agregar unidades de caché o unidades de capacidad de forma independiente, siempre que lo desee.

![Enlace dinámico](media/understand-the-cache/Dynamic-Binding.gif)

Se recomienda que el número de unidades de capacidad sea un múltiplo del número de unidades de caché por motivos de simetría. Por ejemplo, si tienes 4 unidades de caché, habrá un rendimiento más uniforme con 8 unidades de capacidad (relación 1:2) que con 7 o 9 unidades.

## <a name="handling-cache-drive-failures"></a>Control de errores en la unidad de caché

Cuando se produce un error en una unidad de caché, todas las escrituras que aún no se han movido se pierden *en el servidor local*, lo que significa que solo existen en las demás copias (en otros servidores). Al igual que después de cualquier otro error de unidad, los espacios de almacenamiento pueden recuperarse automáticamente, y así lo hacen, mediante la consulta de las copias supervivientes.

Durante un breve período, las unidades de capacidad que estaban enlazadas a la unidad de caché perdida aparecerán en estado incorrecto. Una vez que se ha completado el reenlace de la memoria caché (de forma automática) y la reparación de datos se ha completado (de forma automática), las unidades se reanudarán y aparecerán con estado correcto.

Este escenario es el motivo por el que se necesitan al menos dos unidades de caché por servidor para mantener el rendimiento.

![Control de errores](media/understand-the-cache/Handling-Failure.gif)

Después, puede reemplazar la unidad de caché igual que lo haría con cualquier otra unidad.

   >[!NOTE]
   > Es posible que tenga que apagar para reemplazar de forma segura una NVMe que sea una tarjeta de expansión (AIC) o un factor de forma M.2.

## <a name="relationship-to-other-caches"></a>Relación con otras memorias caché

Hay varias memorias caché no relacionadas en la pila de almacenamiento definida por software de Windows. Entre los ejemplos se incluyen la caché con reescritura de Espacios de almacenamiento y la caché de lectura en memoria del volumen compartido de clúster (CSV).

Con Espacios de almacenamiento directo, no se debería modificar el comportamiento predeterminado de la caché con reescritura de espacios de almacenamiento. Por ejemplo, no se deben usar parámetros como **-WriteCacheSize** en el cmdlet **New-Volume**.

Puede optar por usar la memoria caché de CSV o no: depende de usted. No entra en conflicto con la memoria caché que se describe en este tema de ningún modo. En algunos escenarios, puede mejorar el rendimiento significativamente. Para más información, consulte [Cómo habilitar la caché de CSV](../../failover-clustering/failover-cluster-csvs.md#enable-the-csv-cache-for-read-intensive-workloads-optional).

## <a name="manual-configuration"></a>Configuración manual

Para la mayoría de las implementaciones, no se requiere la configuración manual. En caso de que lo necesite, consulte las secciones siguientes.

Si necesita realizar cambios en el modelo de dispositivo de la caché después de la instalación, edite el documento de componentes de soporte del Servicio de mantenimiento, como se describe en la [introducción al Servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document).

### <a name="specify-cache-drive-model"></a>Especificación del modelo de unidad de caché

En las implementaciones donde todas las unidades tienen el mismo tipo, como las implementaciones de todas unidades NVMe o SSD, no se configura ninguna memoria caché porque Windows no puede distinguir automáticamente las características (por ejemplo, la resistencia de escritura) entre las unidades del mismo tipo.

Si quieres usar unidades de mayor resistencia para almacenar en caché para unidades del mismo tipo que tienen menor resistencia, puedes especificar qué modelo de unidad se va a usar mediante el parámetro **-CacheDeviceModel** del cmdlet **Enable-ClusterS2D**. Una vez que se habilita Espacios de almacenamiento directo, todas las unidades de ese modelo se usarán para el almacenamiento en caché.

   >[!TIP]
   > Asegúrese de que la cadena del modelo coincida exactamente como aparece en la salida de **Get-PhysicalDisk**.

####  <a name="example"></a>Ejemplo

En primer lugar, obtenga una lista de discos físicos:

```PowerShell
Get-PhysicalDisk | Group Model -NoElement
```

A continuación se muestra una salida de ejemplo:

```
Count Name
----- ----
    8 FABRIKAM NVME-1710
   16 CONTOSO NVME-1520
```

A continuación, escriba el siguiente comando, especificando el modelo de dispositivo de la caché:

```PowerShell
Enable-ClusterS2D -CacheDeviceModel "FABRIKAM NVME-1710"
```

Puedes comprobar que las unidades que propones están en uso para almacenamiento en caché al ejecutar **Get-PhysicalDisk** en PowerShell y comprobar que su propiedad **Usage** indica **"Journal"** .

### <a name="manual-deployment-possibilities"></a>Posibilidades de implementación manual

La configuración manual habilita las siguientes posibilidades de implementación:

![Posibilidades de implementación exóticas](media/understand-the-cache/Exotic-Deployment-Possibilities.png)

### <a name="set-cache-behavior"></a>Establecimiento del comportamiento de la caché

Es posible reemplazar el comportamiento predeterminado de la memoria caché. Por ejemplo, puede establecerlo en lecturas de caché en una implementación con todo flash. No es recomendable que modifique el comportamiento a menos que esté seguro de que el predeterminado no es adecuado para su carga de trabajo.

Para reemplazar el comportamiento, use el cmdlet **Set-ClusterStorageSpacesDirect**y sus parámetros **-CacheModeSSD** y **-CacheModeHDD**. El parámetro **CacheModeSSD** establece el comportamiento de la caché al almacenar en caché para las unidades de estado sólido. El parámetro **CacheModeHDD** establece el comportamiento de la caché al almacenar en caché para las unidades de disco duro. Esto se puede realizar en cualquier momento una vez que se haya habilitado Espacios de almacenamiento directo.

Puede usar **Get-ClusterStorageSpacesDirect** para comprobar que se ha establecido el comportamiento.

#### <a name="example"></a>Ejemplo

Primero, obtenga la configuración de Espacios de almacenamiento directo:

```PowerShell
Get-ClusterStorageSpacesDirect
```

A continuación se muestra una salida de ejemplo:

```
CacheModeHDD : ReadWrite
CacheModeSSD : WriteOnly
```

A continuación, haga lo siguiente:

```PowerShell
Set-ClusterStorageSpacesDirect -CacheModeSSD ReadWrite

Get-ClusterS2D
```

A continuación se muestra una salida de ejemplo:

```
CacheModeHDD : ReadWrite
CacheModeSSD : ReadWrite
```

## <a name="sizing-the-cache"></a>Cambio de tamaño de la caché

La memoria caché debe tener un tamaño que permita dar cabida al conjunto de trabajo (los datos que se leen o escriben activamente en un momento determinado) de las aplicaciones y las cargas de trabajo.

Esto resulta especialmente importante en implementaciones híbridas con unidades de disco duro. Si el conjunto de trabajo activo supera el tamaño de la caché, o si el conjunto de trabajo activo se desplaza demasiado rápido, aumentarán los errores de caché de lectura y será necesario mover las escrituras de forma más agresiva, lo que afectará al rendimiento general.

Puede usar la utilidad Monitor de rendimiento (PerfMon.exe) integrada en Windows para inspeccionar el número de errores de la caché. En concreto, puedes comparar el valor de **Cache Miss Reads/sec** del contador **Cluster Storage Hybrid Disk** establecido para la E/S por segundo de lectura general de la implementación. Cada "disco híbrido" se corresponde con una unidad de capacidad.

Por ejemplo, 2 unidades de caché enlazadas a 4 unidades de capacidad dan como resultado 4 instancias de objetos de "disco híbrido" por servidor.

![Supervisión del rendimiento](media/understand-the-cache/PerfMon.png)

No hay ninguna regla universal, pero si faltan demasiadas lecturas en la memoria caché, es posible que el tamaño de la caché sea insuficiente y debas contemplar la posibilidad de agregar unidades de caché para ampliarla. Puede agregar unidades de caché o unidades de capacidad de forma independiente, siempre que lo desee.

## <a name="additional-references"></a>Referencias adicionales

- [Elegir tipos de unidades y de resistencia](choosing-drives.md)
- [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)
- [Requisitos de hardware de Espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md)
