---
ms.assetid: 1368bc83-9121-477a-af09-4ae73ac16789
title: Elección de unidades para Espacios de almacenamiento directo
ms.prod: windows-server
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 07/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: 2ffe34097971f4477f0f536637b49b704678c93e
ms.sourcegitcommit: c40c29683d25ed75b439451d7fa8eda9d8d9e441
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2020
ms.locfileid: "85833374"
---
# <a name="choosing-drives-for-storage-spaces-direct"></a>Elección de unidades para Espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones sobre cómo elegir las unidades de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) para satisfacer los requisitos de rendimiento y capacidad.

## <a name="drive-types"></a>Tipos de unidad

Espacios de almacenamiento directo actualmente funciona con cuatro tipos de unidades:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/pmem-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>PMem</b> hace referencia a la memoria persistente, un nuevo tipo de almacenamiento de baja latencia y alto rendimiento.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>NVMe</b> (Express de memoria no volátil) hace referencia a las unidades de estado sólido que se encuentran directamente en el bus PCIe. Factores de forma común son 2.5" U.2, PCIe Add-In-Card (AIC) y M.2. NVMe ofrece un mayor rendimiento de e/s e/s con una latencia menor que cualquier otro tipo de unidad que se admita hoy, excepto la memoria persistente.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px" >
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>SSD</b> hace referencia a las unidades de estado sólido que se conectan a través de una SAS o SAS convencional.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>HDD</b> hace referencia a unidades de disco duro magnéticas y de rotación que ofrecen una gran capacidad de almacenamiento.
        </td>
    </tr>
</table>

## <a name="built-in-cache"></a>Memoria caché integrada

Espacios de almacenamiento directo incluye una memoria caché de servidor integrada. Se trata de una caché de lectura y escritura grande, persistente y en tiempo real. En implementaciones con varios tipos de unidades, se configura automáticamente para usar todas las unidades del tipo "más rápido". Las unidades restantes se usan para la capacidad.

Para obtener más información, consulte [Understanding the cache in espacios de almacenamiento directo](understand-the-cache.md).

## <a name="option-1--maximizing-performance"></a>Opción 1: Maximizar el rendimiento

Para lograr una latencia de submilisegundos uniforme y predecible a través de lecturas y escrituras aleatorias en los datos, o para lograr un IOPS extremadamente alto (hemos realizado [más de 6 millones](https://www.youtube.com/watch?v=0LviCzsudGY&t=28m)!) o el rendimiento de e/s (hemos realizado [más de 1 TB/s](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=16m50s)!), debe ir "todo-Flash".

Actualmente existen tres formas de hacerlo:

![All-Flash-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/All-Flash-Deployment-Possibilities.png)

1. **Todo NVMe.** El uso de todos las unidades de NVMe proporciona un inigualable rendimiento, incluida la latencia baja más predecible. Si todas las unidades son el mismo modelo, no hay memoria caché alguna. También puedes mezclar modelos NVMe de mayor y menor resistencia y configurar la primera para que realice las escrituras en la memoria caché de la segunda (esto [requiere una configuración específica](understand-the-cache.md#manual-configuration)).

2. **NVMe + SSD.** Al usar la NVMe junto con las unidades SSD, la NVMe realizará escrituras de la memoria caché del SSD de forma automática. Esto permite a la escritura fusionarse con la memoria caché y se eliminarán únicamente si es necesario, para evitar el desgaste en las unidades SSD. Esto proporciona características de escrituras para NVMe, mientras que las lecturas se muestran directamente desde el no menos rápido SSD.

3. **Todas las unidades SSD.** Como ocurre con todos las NVMe, no hay memoria caché si todas las unidades son el mismo modelo. Si mezclas modelos de NVMe de mayor y menor resistencia, puedes configurar la primera para que realice las escrituras en la memoria caché de la segunda (esto [requiere una configuración específica](understand-the-cache.md#manual-configuration)).

   >[!NOTE]
   > Una ventaja de usar todas unidades NVMe o SSD sin memoria caché es que se obtiene una capacidad de almacenamiento utilizable en cada unidad. No se "gasta" capacidad alguna en el almacenamiento en la memoria caché, lo cual puede ser atractivo a pequeña escala.

## <a name="option-2--balancing-performance-and-capacity"></a>Opción 2: Equilibrio de rendimiento y capacidad

En el caso de entornos con una gran variedad de aplicaciones y cargas de trabajo, algunas con requisitos de rendimiento rigurosos y otros que requieren una capacidad de almacenamiento considerable, debe "híbrido" con el almacenamiento en caché de NVMe o SSD para unidades de disco duro de mayor tamaño.

![Hybrid-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/Hybrid-Deployment-Possibilities.png)

1. **NVMe + HDD**. Las unidades NVMe acelerarán el proceso de lectura y escritura al almacenar ambos en la memoria caché. El almacenamiento en la memoria caché de las lecturas permite que las unidades de disco duro se centren en la escritura. El almacenamiento en la memoria caché de la escritura absorbe ráfagas y permite a la escritura fusionarse y moverse solo cuando sea necesario, de una manera serializada artificialmente que maximiza el rendimiento de los discos duros IOPS y la E/S. Esto proporciona características de escrituras de NVMe y para los datos de lectura que se leen con frecuencia o que acaban de leerse y también proporciona características de lectura de NVMe.

2. **SSD + HDD**. Al igual que el ejemplo anterior, el SSD acelerará el proceso de lectura y escritura al almacenar ambos en la memoria caché Esto proporciona características de escrituras de SSD y también proporciona características de lectura de SSD para los datos de lectura que se leen con frecuencia o que acaban de leerse.

    Hay una opción adicional, un poco más inusual: usar unidades de *los tres* tipos.

3. **NVMe + SSD + HDD.** Con las unidades de los tres tipos, la memoria caché de las unidades de NVMe para las SSD y HDD. El atractivo es que puedes crear volúmenes en el SSD y en las unidades de disco duro, en paralelo en el mismo clúster, todos ellos acelerados por NVMe. Los primeros son exactamente igual que en una implementación "completamente en flash" y la segunda opción es exactamente igual que en las implementaciones "híbridas" descritas anteriormente. Conceptualmente, es similar a tener dos grupos, con una administración de capacidad en gran medida independiente, ciclos de error y reparación, etc.

   >[!IMPORTANT]
   > Se recomienda usar la capa de SSD para colocar las cargas de trabajo más sensibles al rendimiento en All-Flash.

## <a name="option-3--maximizing-capacity"></a>Opción 3: Maximizar la capacidad

En el caso de las cargas de trabajo que requieren una gran capacidad y escriben con poca frecuencia, como el archivado, los destinos de copia de seguridad, los almacenamientos de datos o el almacenamiento "en frío", debe combinar algunas SSD para el almacenamiento en caché con muchas unidades de disco duro de mayor tamaño.

![Opciones de implementación para maximizar la capacidad](media/choosing-drives-and-resiliency-types/maximizing-capacity.png)

1. **SSD + HDD**. Las unidades SSD almacenarán las lecturas y escrituras para absorber ráfagas y proporcionar una escritura de SSD, con un movimiento optimizado posterior a los discos duros.

>[!IMPORTANT]
>No se admite la configuración con solo HDD. No se recomienda el almacenamiento en caché de SSD de alta resistencia en SSD de baja resistencia.

## <a name="sizing-considerations"></a>Consideraciones de tamaño

### <a name="cache"></a>Cache

Cada servidor debe tener al menos dos unidades de memoria caché (el mínimo necesario para redundancia). Recomendamos hacer que el número de unidades de capacidad sea un múltiplo del número de unidades de caché. Por ejemplo, si tiene 4 unidades de caché, tendrá un rendimiento más coherente con 8 unidades de capacidad (relación 1:2) que con 7 ó 9.

La memoria caché debe ajustarse para adaptarse al espacio de trabajo de las aplicaciones y las cargas de trabajo, es decir, todos los datos que están leyendo y escribiendo activamente en un momento dado. No hay más requisitos de tamaño caché superiores. En el caso de las implementaciones con HDD, un buen punto de partida es el 10% de la capacidad; por ejemplo, si cada servidor tiene 4 unidades de disco duro de 4 TB = 16 TB de capacidad, 2 x 800 GB SSD = 1,6 TB de caché por servidor. En el caso de las implementaciones All-Flash, especialmente con SSD de [alta resistencia](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/) , puede ser justo empezar más cerca del 5% de la capacidad; por ejemplo, si cada servidor tiene un SSD de 24 x 1,2 tb = 28,8 TB de capacidad, 2 x 750 GB NVMe = 1,5 TB de caché por servidor. Siempre puedes agregar o quitar las unidades de caché para ajustarlas más adelante.

### <a name="general"></a>General

Se recomienda limitar la capacidad de almacenamiento total por servidor a aproximadamente 400 terabytes (TB). Cuanta más capacidad de almacenamiento tenga un servidor, más tiempo se necesitará para resincronizar los datos tras un periodo de inactividad o reinicio, como por ejemplo tras aplicar actualizaciones de software. El tamaño máximo actual por grupo de almacenamiento es 4 petabyte (PB) (4.000 TB) para Windows Server 2019 o 1 petabyte para Windows Server 2016.

## <a name="additional-references"></a>Referencias adicionales

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Conoce la memoria caché de Espacios de almacenamiento directo](understand-the-cache.md)
- [Requisitos de hardware Espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md)
- [Planeación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
- [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)
