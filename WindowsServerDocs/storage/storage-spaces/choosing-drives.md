---
ms.assetid: 1368bc83-9121-477a-af09-4ae73ac16789
title: Selección de unidades para Espacios de almacenamiento directo
ms.prod: windows-server
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 07/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: aae554c87b8a4ac6005ad359bc474026f5f1ed9e
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769413"
---
# <a name="choosing-drives-for-storage-spaces-direct"></a>Selección de unidades para Espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones sobre cómo elegir las unidades de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) para satisfacer los requisitos de rendimiento y capacidad.

## <a name="drive-types"></a>Tipos de unidad

Espacios de almacenamiento directo funciona actualmente con cuatro tipos de unidades:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/pmem-100px.png" alt="Image of PMem (persistent memory)">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>PMem</b> hace referencia a la memoria persistente, un nuevo tipo de almacenamiento de baja latencia y alto rendimiento.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png" alt="Image of NVMe (Non-Volatile Memory Express)">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>NVMe</b> (memoria no volátil rápida) hace referencia a las unidades de estado sólido que se colocan directamente en el bus PCIe. Los factores de forma comunes son 2,5" U.2, AIC (Add-In Card) PCIe y M.2. NVMe ofrece un mayor rendimiento de e/s e/s con una latencia menor que cualquier otro tipo de unidad que se admita hoy, excepto la memoria persistente.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px" >
            <img src="media/understand-the-cache/SSD-100px.png" alt="Image of SSD drive">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>SSD</b> hace referencia a las unidades de estado sólido que se conectan a través de una SAS o SAS convencional.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png" alt="Image of HDD">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>HDD</b> hace referencia a unidades de disco duro magnéticas y de rotación que ofrecen una gran capacidad de almacenamiento.
        </td>
    </tr>
</table>

## <a name="built-in-cache"></a>Memoria caché integrada

Espacios de almacenamiento directo presenta una caché de lado servidor integrada. Se trata de una caché de lectura y escritura grande, persistente y en tiempo real. En implementaciones con varios tipos de unidades, se configura automáticamente para usar todas las unidades del tipo "más rápido". Las unidades restantes se usan para la capacidad.

Para más información, consulte [Descripción de la memoria caché de Espacios de almacenamiento directo](understand-the-cache.md).

## <a name="option-1--maximizing-performance"></a>Opción 1: Maximización del rendimiento

Para lograr una latencia de submilisegundos uniforme y predecible a través de lecturas y escrituras aleatorias en los datos, o para lograr un IOPS extremadamente alto (hemos realizado [más de 6 millones](https://www.youtube.com/watch?v=0LviCzsudGY&t=28m)!) o el rendimiento de e/s (hemos realizado [más de 1 TB/s](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=16m50s)!), debe ir "todo-Flash".

Actualmente existen tres formas de hacerlo:

![Posibilidades de implementación de todo flash](media/choosing-drives-and-resiliency-types/All-Flash-Deployment-Possibilities.png)

1. **Todo NVMe.** El uso de todo NVMe proporciona un rendimiento incomparable, incluida la baja latencia más predecible. Si todas las unidades son el mismo modelo, no hay ninguna caché. También es posible combinar modelos de NVMe de alta y baja resistencia, y configurar el primero para almacenar en caché las escrituras del segundo ([requiere instalación](understand-the-cache.md#manual-configuration)).

2. **NVMe + SSD.** El uso combinado de NVMe y SSD permite que NVMe almacene automáticamente en caché las escrituras en las unidades SSD. De esta manera, las escrituras se fusionan en la caché y se anula su almacenamiento provisional solo cuando es necesario, para reducir el desgaste en las SSD. Este procedimiento proporciona características de escritura de tipo NVMe, mientras que las lecturas se atienden directamente desde las SSD también rápidas.

3. **Todo SSD.** Como con todo NVMe, no hay ninguna caché si todas las unidades tienen el mismo modelo. Si combina modelos de mayor y menor resistencia, puede configurar el primero para almacenar en caché las escrituras del segundo ([requiere instalación](understand-the-cache.md#manual-configuration)).

   >[!NOTE]
   > Una ventaja de usar todo NVMe o todo SSD sin caché es que se obtiene la capacidad de almacenamiento utilizable de todas las unidades. No hay ninguna capacidad "dedicada" al almacenamiento en caché, lo que puede resultar atractivo a menor escala.

## <a name="option-2--balancing-performance-and-capacity"></a>Opción 2: Equilibrio de rendimiento y capacidad

En el caso de entornos con una gran variedad de aplicaciones y cargas de trabajo, algunos con requisitos de rendimiento rigurosos y otros que requieren una capacidad de almacenamiento considerable, debe decantarse por una opción "híbrida" con almacenamiento en caché NVMe o SSD para unidades HDD de mayor tamaño.

![Posibilidades de implementación híbrida](media/choosing-drives-and-resiliency-types/Hybrid-Deployment-Possibilities.png)

1. **NVMe + HDD**. Las unidades NVMe acelerarán las lecturas y escrituras al almacenar en caché ambas. El almacenamiento en caché de las lecturas permite que las unidades HDD se centren en las escrituras. El almacenamiento en caché de las escrituras absorbe las ráfagas y permite que las escrituras se fusionen y se anule su almacenamiento provisional, según sea necesario, de una manera serializada artificialmente que maximice el rendimiento de IOPS y E/S de HDD. Este procedimiento proporciona características de escritura de tipo NVMe y, para los datos leídos con frecuencia o recientemente, también características de lectura de tipo NVMe.

2. **SSD + HDD**. De forma similar a lo anterior, las SSD acelerarán las lecturas y escrituras al almacenar en caché ambas. Como resultado, se proporcionan características de lectura y escritura de tipo SSD para datos leídos con frecuencia o recientemente.

    Hay una opción adicional más exótica: usar unidades de *los tres* tipos.

3. **NVMe + SSD + HDD.** Con las unidades de los tres tipos, las unidades NVMe realizan el almacenamiento en caché de los SSD y HDD. Lo interesante es que puede crear volúmenes en las SSD y volúmenes en las HHD, en paralelo en el mismo clúster, todos ellos acelerados mediante NVMe. Los primeros son exactamente como en una implementación todo flash, y los segundos son exactamente como en las implementaciones "híbridas" descritas anteriormente. Desde un punto de vista conceptual, es como tener dos grupos, con ciclos de reparación, errores y administración de la capacidad en gran medida independientes, etc.

   >[!IMPORTANT]
   > Se recomienda usar el nivel SSD para colocar las cargas de trabajo más sensibles al rendimiento en todo flash.

## <a name="option-3--maximizing-capacity"></a>Opción 3: Maximización de la capacidad

En el caso de las cargas de trabajo que requieren una gran capacidad y escriben con poca frecuencia, como el archivado, los destinos de copia de seguridad, los almacenamientos de datos o el almacenamiento "en frío", debe combinar algunas SSD para el almacenamiento en caché con muchas unidades de disco duro de mayor tamaño.

![Opciones de implementación para maximizar la capacidad](media/choosing-drives-and-resiliency-types/maximizing-capacity.png)

1. **SSD + HDD**. Las SSD almacenarán en caché las lecturas y escrituras para absorber las ráfagas y proporcionarán rendimiento de escritura de tipo SSD, con anulación de almacenamiento provisional optimizada más tarde en las HDD.

>[!IMPORTANT]
>No se admite la configuración con solo HDD. No se recomienda el almacenamiento en caché de SSD de alta resistencia en SSD de baja resistencia.

## <a name="sizing-considerations"></a>Consideraciones de tamaño

### <a name="cache"></a>Cache

Cada servidor debe tener al menos dos unidades de caché (el mínimo necesario para la redundancia). Se recomienda que el número de unidades de capacidad sea un múltiplo del número de unidades de caché. Por ejemplo, si tiene 4 unidades de caché, experimentará un rendimiento más coherente con 8 unidades de capacidad (relación 1:2) que con 7 o 9.

La memoria caché debe ajustarse para adaptarse al espacio de trabajo de las aplicaciones y las cargas de trabajo, es decir, todos los datos que están leyendo y escribiendo activamente en un momento dado. Aparte de eso, no hay ningún requisito de tamaño de la caché. En el caso de las implementaciones con HDD, un buen punto de partida es el 10% de la capacidad; por ejemplo, si cada servidor tiene 4 unidades de disco duro de 4 TB = 16 TB de capacidad, 2 x 800 GB SSD = 1,6 TB de caché por servidor. En el caso de las implementaciones All-Flash, especialmente con SSD de [alta resistencia](https://techcommunity.microsoft.com/t5/storage-at-microsoft/understanding-ssd-endurance-drive-writes-per-day-dwpd-terabytes/ba-p/426024) , puede ser justo empezar más cerca del 5% de la capacidad; por ejemplo, si cada servidor tiene un SSD de 24 x 1,2 tb = 28,8 TB de capacidad, 2 x 750 GB NVMe = 1,5 TB de caché por servidor. Siempre puede agregar o quitar unidades de caché más adelante para realizar los ajustes necesarios.

### <a name="general"></a>General

Se recomienda limitar la capacidad total de almacenamiento por servidor a aproximadamente 400 terabytes (TB). Cuanto mayor sea la capacidad de almacenamiento por servidor, mayor será el tiempo necesario para volver a sincronizar los datos después del tiempo de inactividad o el reinicio, como cuando se aplican actualizaciones de software. El tamaño máximo actual por grupo de almacenamiento es 4 petabyte (PB) (4.000 TB) para Windows Server 2019 o 1 petabyte para Windows Server 2016.

## <a name="additional-references"></a>Referencias adicionales

- [Introducción a Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Conoce la memoria caché de Espacios de almacenamiento directo](understand-the-cache.md)
- [Requisitos de hardware de Espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md)
- [Planeación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
- [Tolerancia a errores y eficacia del almacenamiento](storage-spaces-fault-tolerance.md)
