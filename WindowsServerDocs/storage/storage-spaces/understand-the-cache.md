---
title: Descripción de la memoria caché de Espacios de almacenamiento directo
ms.assetid: 69b1adc0-ee64-4eed-9732-0fb216777992
ms.prod: windows-server
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: f2c2e0435d06c18dbacab4e85db770ba86e654b3
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322347"
---
# <a name="understanding-the-cache-in-storage-spaces-direct"></a>Descripción de la memoria caché de Espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

[Espacios de almacenamiento directo](storage-spaces-direct-overview.md) presenta una memoria caché del lado del servidor integrada para maximizar el rendimiento del almacenamiento. Se trata de una caché de lectura *y* escritura grande, persistente y en tiempo real. La memoria caché se configura automáticamente cuando se habilita Espacios de almacenamiento directo. En la mayoría de los casos, no se necesita administración manual.
El funcionamiento de la memoria caché depende de los tipos de unidades presentes.

En el siguiente vídeo se explica detenidamente cómo el almacenamiento en caché funciona para Espacios de almacenamiento directo, así como otras consideraciones de diseño.

<strong>Consideraciones sobre el diseño de Espacios de almacenamiento directo</strong><br>(20 minutos)<br>
<iframe src="https://channel9.msdn.com/Blogs/windowsserver/Design-Considerations-for-Storage-Spaces-Direct/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="drive-types-and-deployment-options"></a>Tipos de unidades y opciones de implementación

La característica Espacios de almacenamiento directo actualmente funciona con tres tipos de dispositivos de almacenamiento:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            NVMe (memoria no volátil exprés)
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            SSD (unidad de estado sólido) SATA o SAS
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            HDD (unidad de disco duro)
        </td>
    </tr>
</table>

Estos dispositivos se pueden combinar de seis maneras, que se agrupan en dos categorías: "memoria flash" e "híbrida".

### <a name="all-flash-deployment-possibilities"></a>Posibilidades de implementación en memoria flash

Las implementaciones en memoria flash están destinadas a maximizar el rendimiento del almacenamiento y no incluyen unidades de disco duro (HDD) rotacionales.

![All-Flash-Deployment-Possibilities](media/understand-the-cache/All-Flash-Deployment-Possibilities.png)

### <a name="hybrid-deployment-possibilities"></a>Posibilidades de implementación híbrida

Las implementaciones híbridas tienen como objetivo equilibrar el rendimiento y la capacidad o maximizar la capacidad e incluyen unidades de disco duro (HDD) rotacionales.

![Hybrid-Deployment-Possibilities](media/understand-the-cache/Hybrid-Deployment-Possibilities.png)

## <a name="cache-drives-are-selected-automatically"></a>Selección automática de las unidades de caché

En las implementaciones con varios tipos de unidades, la característica Espacios de almacenamiento directo usa automáticamente todas las unidades del tipo "más rápido" para el almacenamiento en caché. Las unidades restantes se usan para capacidad.

Qué tipo es "el más rápido" se determina según la jerarquía siguiente.

![Drive-Type-Hierarchy](media/understand-the-cache/Drive-Type-Hierarchy.png)

Por ejemplo, si tienes unidades SSD y NVMe, la NVMe almacenará en caché para las unidades SSD.

Si tienes unidades SSD y HDD, las SSD almacenarán en caché para las HDD.

   >[!NOTE]
   > Las unidades de caché no aportan capacidad de almacenamiento utilizable. Todos los datos almacenados en la memoria caché también están almacenados en otro lugar (o se almacenarán cuando se muevan). Esto significa que la capacidad de almacenamiento total sin procesar de la implementación es la suma de las unidades de capacidad únicamente.

Cuando todas las unidades son del mismo tipo, no se configura automáticamente ninguna memoria caché. Tienes posibilidad de configurar manualmente unidades de mayor resistencia para caché en el caso de unidades de menor resistencia del mismo tipo. Consulta la sección [Configuración manual](#manual-configuration) para aprender a hacerlo.

   >[!TIP]
   > En el caso de implementaciones del tipo todo NVMe o todo SSD, en especial a escala muy pequeña, si no "dedicas" ninguna unidad a memoria caché, puedes mejorar significativamente la eficiencia del almacenamiento.

## <a name="cache-behavior-is-set-automatically"></a>Establecimiento automático del comportamiento de la memoria caché

El comportamiento de la caché se determina automáticamente en función de los tipos de unidades para las que se almacena en caché. Al almacenar en caché para unidades de estado sólido (como el almacenamiento en caché de NVMe para SSD), solo se almacenan en caché las escrituras. Al almacenar en caché para unidades de disco duro (por ejemplo, el almacenamiento en caché de SSD para HDD), se almacenan en caché las lecturas y las escrituras.

![Cache-Read-Write-Behavior](media/understand-the-cache/Cache-Read-Write-Behavior.png)

### <a name="write-only-caching-for-all-flash-deployments"></a>Almacenamiento en caché de solo escritura para implementaciones en memoria flash

Al almacenar en caché para unidades de estado sólido (NVMe o SSD), solo se almacenan en caché las escrituras. Esto reduce el desgaste de las unidades de capacidad porque muchas escrituras y reescrituras pueden fusionarse en la memoria caché y luego se mueven según sea necesario, lo que reduce el tráfico acumulativo hacia las unidades de capacidad y prolonga su duración. Por este motivo, recomendamos seleccionar unidades de [mayor de resistencia, optimizadas para escritura](http://whatis.techtarget.com/definition/DWPD-device-drive-writes-per-day) para la memoria caché. Las unidades de capacidad pueden tener razonablemente una menor resistencia de escritura.

Dado que las lecturas no afectan significativamente a la vida útil de la memoria flash, y que las unidades de estado sólido universalmente ofrecen baja latencia de lectura, las lecturas no se almacenan en caché, sino que se proporcionan directamente desde las unidades de capacidad (excepto cuando los datos se escribieron tan recientemente que aún no se han movido). Esto permite que la memoria caché esté dedicada totalmente a escrituras, lo que maximiza su eficacia.

Como resultado, las características de escritura, como la latencia de escritura, están determinadas por las unidades de caché, mientras que las características de lectura están determinadas por las unidades de capacidad. Ambos casos son coherentes, predecibles y uniformes.

### <a name="readwrite-caching-for-hybrid-deployments"></a>Almacenamiento en caché de lecturas y escrituras para implementaciones híbridas

Al almacenar en caché para unidades de disco duro (HDD), las lecturas *y* las escrituras se almacenan en caché, con el propósito de ofrecer una latencia similar a la memoria flash (suele ser ~10 veces mejor) para ambas. La caché de lectura almacena datos tanto leídos recientemente como leídos con frecuencia para permitir un acceso rápido y minimizar el tráfico aleatorio hacia las unidades de disco duro. (Debido a los retrasos por búsqueda y rotación, la latencia y la pérdida de tiempo que provoca el acceso aleatorio a una unidad de disco duro es importante). Las escrituras se almacenan en caché para absorber las ráfagas y, al igual que antes, para fusionar las escrituras y las reescrituras y minimizar el tráfico acumulativo hacia las unidades de capacidad.

Espacios de almacenamiento directo implementa un algoritmo que elimina la aleatoriedad de las escrituras antes de moverlas, para emular un modelo de E/S en disco que parezca secuencial aunque la E/S real proveniente de la carga de trabajo (por ejemplo, máquinas virtuales) sea aleatoria. Esto maximiza la E/S por segundo (IOPS) y el rendimiento en las unidades de disco duro.

### <a name="caching-in-deployments-with-drives-of-all-three-types"></a>Almacenamiento en caché en implementaciones con unidades de los tres tipos

Cuando hay unidades de los tres tipos, las unidades NVMe proporcionan almacenamiento en caché para SSD y HDD. El comportamiento es tal como se describió anteriormente: para SSD, solo se almacenan en caché las escrituras y para HDD, se almacenan en caché las lecturas y las escrituras. La carga del almacenamiento en caché para las unidades de disco duro se distribuye uniformemente entre las unidades de caché. 

## <a name="summary"></a>Resumen

En la siguiente tabla se resumen qué unidades se usan para almacenar en caché, cuáles se usan para capacidad y cuál es el comportamiento del almacenamiento en caché para cada posibilidad de implementación.

| Implementación     | Unidades de caché                        | Unidades de capacidad | Comportamiento de la caché (predeterminado)  |
| -------------- | ----------------------------------- | --------------- | ------------------------- |
| Todas las unidades NVMe         | Ninguna (opcional: configurar manualmente) | NVMe            | Solo escritura (si está configurado)  |
| Todas las unidades SSD          | Ninguna (opcional: configurar manualmente) | SSD             | Solo escritura (si está configurado)  |
| NVMe + SSD       | NVMe                                | SSD             | Solo escritura                  |
| NVMe + HDD       | NVMe                                | HDD             | Lectura + escritura                |
| SSD + HDD        | SSD                                 | HDD             | Lectura + escritura                |
| NVMe + SSD + HDD | NVMe                                | SSD + HDD       | Lectura + escritura para HDD, solo escritura para SSD  |

## <a name="server-side-architecture"></a>Arquitectura del lado del servidor

La memoria caché se implementa en el nivel de unidad: las unidades de caché individuales dentro de un servidor están enlazadas a una o varias unidades de capacidad en el mismo servidor.

Dado que la memoria caché está debajo del resto de la pila de almacenamiento definido por software de Windows, no tiene ni necesita el reconocimiento de conceptos como espacios de almacenamiento o tolerancia a errores. Se puede considerar como la creación de unidades "híbridas" (una parte memoria flash y otra parte disco) que se presentan para Windows. Al igual que con una unidad híbrida real, el movimiento en tiempo real de los datos fríos y calientes entre las partes más rápidas y más lentas de los medios físicos es casi invisible en el exterior.

Dado que la resistencia de la característica Espacios de almacenamiento directo ocurre, como mínimo, en el nivel de servidor (es decir, las copias de datos siempre se escriben en servidores diferentes, a lo sumo una copia por servidor), los datos que están en la memoria caché aprovechan la misma resistencia que los datos que no están en la caché.

![Cache-Server-Side-Architecture](media/understand-the-cache/Cache-Server-Side-Architecture.png)

Por ejemplo, al usar la creación de reflejo triple, se escriben tres copias de cualquier dato en diferentes servidores, en el lugar donde llegan en la caché. Independientemente de si más adelante se mueven o no, siempre existen tres copias.

## <a name="drive-bindings-are-dynamic"></a>Los enlaces de unidad son dinámicos

El enlace entre las unidades de memoria caché y capacidad puede tener cualquier relación, desde 1:1 hasta 1:12 y más aún. Se ajusta dinámicamente siempre que se agregan o quitan unidades, por ejemplo, cuando se escala verticalmente o tras errores. Esto significa que puedes agregar unidades de caché o de capacidad de forma independiente, cuando quieras.

![Dynamic-Binding](media/understand-the-cache/Dynamic-Binding.gif)

Recomendamos hacer que el número de unidades de capacidad sea un múltiplo del número de unidades de caché por cuestiones de simetría. Por ejemplo, si tienes 4 unidades de caché, habrá un rendimiento más uniforme con 8 unidades de capacidad (relación 1:2) que con 7 o 9 unidades.

## <a name="handling-cache-drive-failures"></a>Controlar los errores de las unidades de caché

Cuando se produce un error en una unidad de caché, todas las escrituras que aún no se han movido se pierden *en el servidor local*, lo que significa que solo existen en las demás copias (en otros servidores). Al igual que después de un error de cualquier otra unidad, Espacios de almacenamiento puede (y efectivamente lo hace) recuperar automáticamente consultando las copias que quedan.

Durante un período breve, las unidades de capacidad que estaban enlazadas a la unidad de caché perdida aparecerán con estado incorrecto. Una vez que se vuelva a enlazar la memoria caché (automático) y se complete la reparación de datos (automático), dichas unidades volverán a mostrarse con estado correcto.

El siguiente escenario muestra por qué se necesitan al menos dos unidades de caché por servidor para conservar el rendimiento.

![Handling-Failure](media/understand-the-cache/Handling-Failure.gif)

De este modo, se puede reemplazar la unidad de caché de la misma manera que cualquier otra unidad.

   >[!NOTE]
   > Tal vez tengas que apagar el equipo para reemplazar de forma segura la unidad NVMe que tiene un factor de forma M.2 o tarjeta de complemento (AIC).

## <a name="relationship-to-other-caches"></a>Relación con otras memorias caché

Hay varias memorias caché no relacionadas en la pila de almacenamiento definido por software de Windows. Algunos ejemplos incluyen la caché con reescritura de Espacios de almacenamiento, la caché de lectura en memoria de Volumen compartido de clúster (CSV).

Con Espacios de almacenamiento directos, no se debe modificar el comportamiento predeterminado de la caché con reescritura de Espacios de almacenamiento. Por ejemplo, los parámetros como **-WriteCacheSize** del cmdlet **New-Volume** no deben usarse.

Puedes optar por usar o no la caché de CSV según tus preferencias. Está desactivada de forma predeterminada en Espacios de almacenamiento directo, pero no entra en conflicto con la nueva caché descrita en este tema en absoluto. En determinados escenarios puede proporcionar mejoras importantes en el rendimiento. Para obtener más información, consulta [How to Enable CSV Cache](../../failover-clustering/failover-cluster-csvs.md#enable-the-csv-cache-for-read-intensive-workloads-optional) (Cómo habilitar la caché de CSV).

## <a name="manual-configuration"></a>Configuración manual

Para la mayoría de las implementaciones, no se requiere de configuración manual. En caso de que lo necesite, consulte las secciones siguientes. 

Si necesita realizar cambios en el modelo de dispositivo de caché después de la instalación, edite el documento de componentes de soporte de Servicio de mantenimiento, como se describe en [servicio de mantenimiento información general](../../failover-clustering/health-service-overview.md#supported-components-document).

### <a name="specify-cache-drive-model"></a>Especificar el modelo de unidad de caché

En las implementaciones donde todas las unidades tienen el mismo tipo, como las implementaciones de todas unidades NVMe o SSD, no se configura ninguna memoria caché porque Windows no puede distinguir automáticamente las características (por ejemplo, la resistencia de escritura) entre las unidades del mismo tipo.

Si quieres usar unidades de mayor resistencia para almacenar en caché para unidades del mismo tipo que tienen menor resistencia, puedes especificar qué modelo de unidad se va a usar mediante el parámetro **-CacheDeviceModel** del cmdlet **Enable-ClusterS2D**. Una vez habilitada la característica Espacios de almacenamiento directo, todas las unidades de ese modelo se usarán para almacenamiento en caché.

   >[!TIP]
   > Asegúrate de que coincida con la cadena de modelo tal y como aparece en la salida de **Get-PhysicalDisk**.

####  <a name="example"></a>Ejemplo

En primer lugar, obtenga una lista de discos físicos:

```PowerShell
Get-PhysicalDisk | Group Model -NoElement
```

Este es un ejemplo de los resultados:

```
Count Name
----- ----
    8 FABRIKAM NVME-1710
   16 CONTOSO NVME-1520
```

A continuación, escriba el siguiente comando, especificando el modelo de dispositivo de caché:

```PowerShell
Enable-ClusterS2D -CacheDeviceModel "FABRIKAM NVME-1710"
```

Puedes comprobar que las unidades que propones están en uso para almacenamiento en caché al ejecutar **Get-PhysicalDisk** en PowerShell y comprobar que su propiedad **Usage** indica **"Journal"** .

### <a name="manual-deployment-possibilities"></a>Posibilidades de implementación manual

La configuración manual ofrece las posibilidades de implementación siguientes:

![Exotic-Deployment-Possibilities](media/understand-the-cache/Exotic-Deployment-Possibilities.png)

### <a name="set-cache-behavior"></a>Establecer el comportamiento de la memoria caché

Es posible invalidar el comportamiento predeterminado de la memoria caché. Por ejemplo, puedes establecerlo para que almacene en caché las lecturas incluso en una implementación íntegramente en memoria flash. No es conveniente modificar el comportamiento a menos que estés seguro de que el valor predeterminado no es adecuado para la carga de trabajo.

Para invalidar el comportamiento, use el cmdlet **set-ClusterStorageSpacesDirect** y sus parámetros **-CacheModeSSD** y **-CacheModeHDD** . El parámetro **CacheModeSSD** establece el comportamiento de la memoria caché al almacenar en caché para unidades de estado sólido. El parámetro **CacheModeHDD** establece el comportamiento de la memoria caché al almacenar en caché para unidades de disco duro. Esto puede hacerse en cualquier momento después de habilitar la característica Espacios de almacenamiento directo.

Puede usar **Get-ClusterStorageSpacesDirect** para comprobar que el comportamiento está establecido.

#### <a name="example"></a>Ejemplo

En primer lugar, obtenga la configuración de Espacios de almacenamiento directo:

```PowerShell
Get-ClusterStorageSpacesDirect
```

Este es un ejemplo de los resultados:

```
CacheModeHDD : ReadWrite
CacheModeSSD : WriteOnly
```

Luego, haga lo siguiente:

```PowerShell
Set-ClusterStorageSpacesDirect -CacheModeSSD ReadWrite

Get-ClusterS2D
```

Este es un ejemplo de los resultados:

```
CacheModeHDD : ReadWrite
CacheModeSSD : ReadWrite
```

## <a name="sizing-the-cache"></a>Cambiar el tamaño de la memoria caché

La memoria caché debe tener un tamaño que permita dar cabida al conjunto de trabajo (los datos que se leen o escriben activamente en un momento determinado) de las aplicaciones y las cargas de trabajo.

Esto es especialmente importante en las implementaciones híbridas con unidades de disco duro. Si el conjunto de trabajo activo supera el tamaño de la caché, o si el conjunto de trabajo activo se desplaza demasiado rápido, aumentarán los errores de caché de lectura y será necesario mover las escrituras de forma más agresiva, lo que afectará al rendimiento general.

Puedes usar la utilidad integrada Monitor de rendimiento (PerfMon.exe) de Windows para inspeccionar la tasa de errores de caché. En concreto, puedes comparar el valor de **Cache Miss Reads/sec** del contador **Cluster Storage Hybrid Disk** establecido para la E/S por segundo de lectura general de la implementación. Cada "disco híbrido" corresponde a una unidad una capacidad.

Por ejemplo, 2 unidades de caché enlazadas a 4 unidades de capacidad tienen como resultado 4 instancias de objeto "Disco híbrido" por servidor.

![Performance-Monitor](media/understand-the-cache/PerfMon.png)

No hay ninguna regla universal, pero si faltan demasiadas lecturas en la memoria caché, es posible que el tamaño de la caché sea insuficiente y debas contemplar la posibilidad de agregar unidades de caché para ampliarla. Puedes agregar unidades de caché o de capacidad de forma independiente cuando quieras.

## <a name="see-also"></a>Vea también

- [Elección de unidades y tipos de resistencia](choosing-drives.md)
- [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)
- [Requisitos de hardware Espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md)
