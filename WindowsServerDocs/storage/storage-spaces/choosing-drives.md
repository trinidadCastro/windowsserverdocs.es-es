---
ms.assetid: 1368bc83-9121-477a-af09-4ae73ac16789
title: Elegir las unidades para Espacios de almacenamiento directo
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/19/2019
ms.localizationpriority: medium
ms.openlocfilehash: 21ba41f636c95660d16055908f6bef857b0f3608
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323287"
---
# <a name="choosing-drives-for-storage-spaces-direct"></a>Elegir las unidades para Espacios de almacenamiento directo

>Se aplica a: Windows 2019, Windows Server 2016

En este tema encontrarás instrucciones sobre cómo elegir las unidades para [Espacios de almacenamiento directo](storage-spaces-direct-overview.md) para satisfacer los requisitos de rendimiento y capacidad.

## <a name="drive-types"></a>Tipos de unidad

La característica Espacios de almacenamiento directo actualmente funciona con tres tipos de unidades:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>NVMe</b> (memoria no volátil exprés) hace referencia a las unidades de estado sólido que se encuentran directamente en el bus PCIe. Factores de forma común son 2.5" U.2, PCIe Add-In-Card (AIC) y M.2. NVMe ofrece un mayor rendimiento IOPS y E/S con una menor latencia que cualquier otro tipo de unidad admitida actualmente.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px" >
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>SSD</b> hace referencia a las unidades de estado sólido que se conectan mediante unidades SATA o SAS convencionales.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>HDD</b> hace referencia a las unidades de disco duro magnéticas y rotacionales que ofrecen una amplia capacidad de almacenamiento.
        </td>
    </tr>
</table>

## <a name="built-in-cache"></a>Caché integrada

Espacios de almacenamiento directo presenta una memoria caché integrada del lado servidor. Se trata de una memoria caché de lectura y escritura grande, persistente y en tiempo real. En las implementaciones con varios tipos de unidades, se configura automáticamente para usar todas las unidades del tipo "más rápido". Las unidades restantes se usan para capacidad.

Para obtener más información, echa un vistazo a [Descripción de la memoria caché de Espacios de almacenamiento directo](understand-the-cache.md).

## <a name="option-1--maximizing-performance"></a>Opción 1: Maximizar el rendimiento

Si deseas lograr una latencia predecible y uniforme de milisegundos en las lecturas y escrituras aleatorias de todos los datos, o para obtener IOPS muy altas (hemos realizado [más de seis millones](https://www.youtube.com/watch?v=0LviCzsudGY&t=28m)) o de operaciones de E/S (hemos realizado [más de 1 TB/s](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=16m50s)), deberías usar "todo flash".

Esto se puede hacer de tres maneras:

![All-Flash-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/All-Flash-Deployment-Possibilities.png)

1. **Todo NVMe.** El uso de todos las unidades de NVMe proporciona un inigualable rendimiento, incluida la latencia baja más predecible. Si todas las unidades son el mismo modelo, no hay memoria caché alguna. También puedes mezclar modelos NVMe de mayor y menor resistencia y configurar la primera para que realice las escrituras en la memoria caché de la segunda (esto [requiere una configuración específica](understand-the-cache.md#manual-configuration)).

2. **NVMe + SSD.** Al usar la NVMe junto con las unidades SSD, la NVMe realizará escrituras de la memoria caché del SSD de forma automática. Esto permite a la escritura fusionarse con la memoria caché y se eliminarán únicamente si es necesario, para evitar el desgaste en las unidades SSD. Esto proporciona características de escrituras para NVMe, mientras que las lecturas se muestran directamente desde el no menos rápido SSD.

3. **Todas las SSD.** Como ocurre con todos las NVMe, no hay memoria caché si todas las unidades son el mismo modelo. Si mezclas modelos de NVMe de mayor y menor resistencia, puedes configurar la primera para que realice las escrituras en la memoria caché de la segunda (esto [requiere una configuración específica](understand-the-cache.md#manual-configuration)).

   >[!NOTE]
   > Una ventaja de usar todas unidades NVMe o SSD sin memoria caché es que se obtiene una capacidad de almacenamiento utilizable en cada unidad. No se "gasta" capacidad alguna en el almacenamiento en la memoria caché, lo cual puede ser atractivo a pequeña escala.

## <a name="option-2--balancing-performance-and-capacity"></a>Opción 2: Equilibrio de rendimiento y capacidad

Para entornos con una variedad de aplicaciones y cargas de trabajo, algunos con requisitos de rendimiento estrictos y otros con requisitos de capacidad de almacenamiento considerable, deberías elegir "híbrido" con almacenamiento en caché NVMe o SSD para unidades de disco duro más grandes.

![Hybrid-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/Hybrid-Deployment-Possibilities.png)

1. **NVMe + HDD**. Las unidades NVMe acelerarán el proceso de lectura y escritura al almacenar ambos en la memoria caché. El almacenamiento en la memoria caché de las lecturas permite que las unidades de disco duro se centren en la escritura. El almacenamiento en la memoria caché de la escritura absorbe ráfagas y permite a la escritura fusionarse y moverse solo cuando sea necesario, de una manera serializada artificialmente que maximiza el rendimiento de los discos duros IOPS y la E/S. Esto proporciona características de escrituras de NVMe y para los datos de lectura que se leen con frecuencia o que acaban de leerse y también proporciona características de lectura de NVMe.

2. **SSD + HDD**. Al igual que el ejemplo anterior, el SSD acelerará el proceso de lectura y escritura al almacenar ambos en la memoria caché Esto proporciona características de escrituras de SSD y también proporciona características de lectura de SSD para los datos de lectura que se leen con frecuencia o que acaban de leerse.

    Hay una opción adicional, un poco más inusual: usar unidades de *los tres* tipos.

3. **NVMe + SSD + HDD.** Con unidades de los tres tipos, las unidades NVMe almacenan en caché tanto los SSD como las HDD. El atractivo es que puedes crear volúmenes en el SSD y en las unidades de disco duro, en paralelo en el mismo clúster, todos ellos acelerados por NVMe. Los primeros son exactamente igual que en una implementación "completamente en flash" y la segunda opción es exactamente igual que en las implementaciones "híbridas" descritas anteriormente. Conceptualmente, es similar a tener dos grupos, con una administración de capacidad en gran medida independiente, ciclos de error y reparación, etc.

   >[!IMPORTANT]
   > Te recomendamos que uses la capa de SSD para colocar las cargas de trabajo más dependientes del rendimiento en todo flash.

## <a name="option-3--maximizing-capacity"></a>Opción 3: Maximizar la capacidad

Para las cargas de trabajo que requieren una gran capacidad y escriben con poca frecuencia, como los destinos de archivo, copia de seguridad, los almacenes de datos o el almacenamiento "frío", debes combinar algunos SSD para almacenar en caché con muchas HDD más grandes para capacidad.

![Opciones de implementación para maximizar la capacidad](media/choosing-drives-and-resiliency-types/maximizing-capacity.png)

1. **SSD + HDD**. Las unidades SSD almacenarán las lecturas y escrituras para absorber ráfagas y proporcionar una escritura de SSD, con un movimiento optimizado posterior a los discos duros.

>[!IMPORTANT]
>No se admite la configuración con solo HDD. No se recomienda el almacenamiento en caché de SSD de alta resistencia en SSD de baja resistencia.

## <a name="sizing-considerations"></a>Consideraciones sobre el tamaño

### <a name="cache"></a>Memoria caché

Cada servidor debe tener al menos dos unidades de memoria caché (el mínimo necesario para redundancia). Recomendamos hacer que el número de unidades de capacidad sea un múltiplo del número de unidades de caché. Por ejemplo, si tienes 4 unidades de caché, habrá un rendimiento más coherente con 8 unidades de capacidad (relación 1:2) que con 7 o 9 unidades.

La memoria caché debe ajustarse para adaptarse al espacio de trabajo de las aplicaciones y las cargas de trabajo, es decir, todos los datos que están leyendo y escribiendo activamente en un momento dado. No hay más requisitos de tamaño caché superiores. En el caso de las implementaciones con HDD, un buen punto de partida es el 10% de la capacidad; por ejemplo, si cada servidor tiene 4 unidades de disco duro de 4 TB = 16 TB de capacidad, 2 x 800 GB SSD = 1,6 TB de caché por servidor. En el caso de las implementaciones All-Flash, especialmente con SSD de [alta resistencia](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/) , puede ser justo empezar más cerca del 5% de la capacidad; por ejemplo, si cada servidor tiene un SSD de 24 x 1,2 tb = 28,8 TB de capacidad, 2 x 750 GB NVMe = 1,5 TB de caché por servidor. Siempre puedes agregar o quitar las unidades de caché para ajustarlas más adelante.

### <a name="general"></a>General

Se recomienda limitar la capacidad de almacenamiento total por servidor a aproximadamente 400 terabytes (TB). Cuanta más capacidad de almacenamiento tenga un servidor, más tiempo se necesitará para resincronizar los datos tras un periodo de inactividad o reinicio, como por ejemplo tras aplicar actualizaciones de software. El tamaño máximo actual por grupo de almacenamiento es 4 petabyte (PB) (4.000 TB) para Windows Server 2019 o 1 petabyte para Windows Server 2016.

## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Descripción de la memoria caché en Espacios de almacenamiento directo](understand-the-cache.md)
- [Requisitos de hardware Espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md)
- [Planeación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
- [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)
