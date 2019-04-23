---
title: Consideraciones de simetría de unidad para espacios de almacenamiento directo
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866886"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>Consideraciones de simetría de unidad para espacios de almacenamiento directo 

> Se aplica a: Windows Server 2019, Windows Server 2016

[Espacios de almacenamiento directo](storage-spaces-direct-overview.md) funciona mejor cuando cada servidor tiene exactamente las mismas unidades.

En realidad, reconocemos que no siempre resulta práctico: Espacios de almacenamiento directo está diseñado para ejecutarse durante años y escalar a medida que crecen las necesidades de su organización. Hoy en día, puede comprar los 3 TB "espacioso" unidades de disco duro; próximo año, puede que sea imposible encontrar aquellas es pequeño. Por lo tanto, se admite cierta cantidad de opción de mezclar y combinar.

Este tema explica las restricciones y proporciona ejemplos de configuraciones compatibles y no compatibles.

## <a name="constraints"></a>Restricciones

### <a name="type"></a>Tipo

Todos los servidores deben tener el mismo [tipos de unidades](choosing-drives.md#drive-types).

Por ejemplo, si un servidor tiene NVMe, deberían *todas* tiene NVMe.

### <a name="number"></a>Número

Todos los servidores deben tener el mismo número de unidades de cada tipo.

Por ejemplo, si un servidor tiene seis SSD, deben *todas* tiene seis SSD.

   > [!NOTE]
   > No pasa nada por el número de unidades para diferir temporalmente durante los errores o al agregar o quitar unidades.

### <a name="model"></a>Modelo

Se recomienda usar las unidades del mismo modelo y la versión de firmware, siempre que sea posible. Si no es posible, seleccione cuidadosamente las unidades que sean lo más parecidas posible. Se desaconseja mezclar y combinar las unidades del mismo tipo con características de rendimiento o resistencia claramente diferentes (a menos que una memoria caché y la otra es la capacidad) ya que distribuye uniformemente E/S espacios de almacenamiento directo y no restrinja por modelo .

   > [!NOTE]
   > No pasa nada por unidades SATA y SAS similar combinar y hacer coincidir.

### <a name="size"></a>Tamaño

Se recomienda usar las unidades de los mismos tamaños siempre que sea posible. Con unidades de capacidad de tamaños diferentes puede provocar cierta capacidad inutilizable y utiliza unidades de caché de diferentes tamaños no puede mejorar el rendimiento de la caché. Consulte la sección siguiente para obtener más información.

   > [!WARNING]
   > Diferentes tamaños de unidades de capacidad a través de servidores pueden dar lugar a pérdida de capacidad.

## <a name="understand-capacity-imbalance"></a>Descripción: el desequilibrio de capacidad

Espacios de almacenamiento directo es sólido para desequilibrio de capacidad en las unidades y entre servidores. Incluso si el desequilibrio es grave, todo lo que seguirá funcionando. Sin embargo, dependiendo de varios factores, capacidad que no está disponible en todos los servidores no pueden utilizable.

Para ver por qué ocurre esto, considere la siguiente ilustración simplificada. Cada cuadro coloreado representa una copia de datos reflejadas. Por ejemplo, los cuadros de marcaron A, un ' y un '' son tres copias de los mismos datos. Dispondrá de tolerancia a errores de servidor, estas copias *debe* se almacenan en servidores diferentes.

### <a name="stranded-capacity"></a>Pérdida de capacidad

Cuando se dibuja, (10 TB) de servidor 1 y 2 (10 TB) de servidor están llenos. Servidor 3 tiene unidades más grandes, por lo tanto, su capacidad total es más grande (15 TB). Sin embargo almacenar más datos triple 3 Server requeriría copias en el servidor 1 y 2 de servidor, que ya están llenos. No se puede usar la capacidad restante de 5 TB en Server 3: es *"en desuso"* capacidad.

![Capacidad triple, tres servidores en desuso](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>Ubicación óptima

Por el contrario, con cuatro servidores de 10 TB, 10 TB, 10 TB y 15 TB de capacidad y resistencia de reflejo triple, lo *es* posible colocar las copias de una manera que usa toda la capacidad disponible, como dibujado válida. Siempre que sea posible, el asignador de espacios de almacenamiento directo buscarán y usarán la colocación óptima, no dejando pérdida de capacidad.

![Triple, cuatro servidores, no hay pérdida de capacidad](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

El número de servidores, la resistencia, la gravedad de desequilibrio de la capacidad y otros factores afectan a si hay pérdida de capacidad. **La regla general más prudente es suponer que se garantiza que solo la capacidad disponible en todos los servidores que se pueda usar.**

## <a name="understand-cache-imbalance"></a>Descripción: el desequilibrio de caché

Espacios de almacenamiento directo es sólido para desequilibrio de la memoria caché en las unidades y entre servidores. Incluso si el desequilibrio es grave, todo lo que seguirá funcionando. Además, espacios de almacenamiento directo siempre utiliza toda la memoria caché disponible al máximo.

Sin embargo, con unidades de caché de diferentes tamaños puede no mejorar el rendimiento de la caché uniformemente o predecible: solo E/S a [unidad enlaces](understand-the-cache.md#server-side-architecture) con mayor caché unidades pueden ver un mejor rendimiento. Espacios de almacenamiento directo distribuye E/S uniformemente entre los enlaces y no restrinja en proporción de capacidad de memoria caché.

![Desequilibrio de la memoria caché](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Consulte [descripción de la memoria caché](understand-the-cache.md) para obtener más información sobre los enlaces de la memoria caché.

## <a name="example-configurations"></a>Configuraciones de ejemplo

Estas son algunas configuraciones admitidas y no admitidas:

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-between-servers"></a>![admitido](media/drive-symmetry-considerations/supported.png) Compatible: diferentes modelos entre servidores

Los dos primeros servidores utilizan modelo NVMe "X" pero el tercer servidor usa el modelo de NVMe "Z", lo que es muy similar.

| Servidor 1                    | Servidor 2                    | Servidor 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe modelo X (caché)    | 2 x NVMe modelo X (caché)    | 2 x NVMe modelo Z (caché)    |
| 10 veces el modelo SSD Y (capacidad) | 10 veces el modelo SSD Y (capacidad) | 10 veces el modelo SSD Y (capacidad) |

Esto se admite.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-within-server"></a>![admitido](media/drive-symmetry-considerations/supported.png) Compatible: diferentes modelos en el servidor

Cada servidor utiliza una mezcla diferentes de modelos de la unidad de disco duro "Y" y "Z", que son muy similares. Cada servidor tiene 10 unidades de disco duro total.

| Servidor 1                   | Servidor 2                   | Servidor 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD modelo X (caché)    | 2 x SSD modelo X (caché)    | 2 x SSD modelo X (caché)    |
| 7 x Y de modelo de unidad de disco duro (capacidad) | 5 x Y de modelo de unidad de disco duro (capacidad) | 1 x Y de modelo de unidad de disco duro (capacidad) |
| 3 x HDD modelo Z (capacidad) | 5 x HDD modelo Z (capacidad) | 9 x HDD modelo Z (capacidad) |

Esto se admite.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-across-servers"></a>![admitido](media/drive-symmetry-considerations/supported.png) Compatible: diferentes tamaños entre servidores

Los dos primeros servidores usan la unidad de disco duro de 4 TB sino el tercer servidor muy similar 6 TB HDD.

| Servidor 1                | Servidor 2                | Servidor 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (caché) | 2 x 800 GB NVMe (caché) | 2 x 800 GB NVMe (caché) |
| 4 x 4 TB HDD (capacidad) | 4 x 4 TB HDD (capacidad) | 4 x 6 TB HDD (capacidad) |

Esto es compatible, aunque dará lugar a pérdida de capacidad.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-within-server"></a>![admitido](media/drive-symmetry-considerations/supported.png) Compatible: diferentes tamaños de servidor

Todos los servidores de usa una mezcla diferente de 1,2 TB y SSD de TB 1.6 muy similar. Cada servidor tiene 4 SSD total.

| Servidor 1                 | Servidor 2                 | Servidor 3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1,2 TB SSD (caché)   | 2 x 1,2 TB SSD (caché)   | 4 x 1,2 TB SSD (caché)   |
| 1 x 1,6 TB SSD (caché)   | 2 x 1,6 TB SSD (caché)   | -                        |
| 20 x 4 TB HDD (capacidad) | 20 x 4 TB HDD (capacidad) | 20 x 4 TB HDD (capacidad) |

Esto se admite.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-types-of-drives-across-servers"></a>![no admitido](media/drive-symmetry-considerations/unsupported.png) No se admite: diferentes tipos de unidades entre servidores

Servidor 1 tiene NVMe, pero los otros no.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (caché)    | -                   | -                   |
| -                   | 6 x SSD (caché)     | 6 x SSD (caché)     |
| 18 x HDD (capacidad) | 18 x HDD (capacidad) | 18 x HDD (capacidad) |

No se admite. Los tipos de unidades de disco deben ser el mismo en todos los servidores.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-number-of-each-type-across-servers"></a>![no admitido](media/drive-symmetry-considerations/unsupported.png) No se admite: un número diferente de cada tipo a través de servidores

Servidor 3 tiene unidades más que los demás.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (caché)    | 2 x NVMe (caché)    | 4 x NVMe (caché)    |
| 10 x HDD (capacidad) | 10 x HDD (capacidad) | 20 x HDD (capacidad) |

No se admite. El número de unidades de cada tipo debe ser el mismo en todos los servidores.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-only-hdd-drives"></a>![no admitido](media/drive-symmetry-considerations/unsupported.png) No se admite: solo las unidades de disco duro

Todos los servidores tengan sólo las unidades de disco duro conectadas.

|Servidor 1|Servidor 2|Servidor 3|
|-|-|-| 
|18 x HDD (capacidad) |18 x HDD (capacidad)|18 x HDD (capacidad)|

No se admite. Deberá agregar un mínimo de dos unidades de caché (NvME o SSD) asociado a cada uno de los servidores.

## <a name="summary"></a>Resumen

Para recapitular, todos los servidores del clúster deben tener los mismos tipos de unidades de disco y el mismo número de cada tipo. Es compatible con los tamaños de unidad según sea necesario, con las consideraciones anteriores y los modelos de la unidad de combinar y hacer coincidir.

| restricción                               |               |
|------------------------------------------|---------------|
| Mismos tipos de unidades en cada servidor     | **Obligatorio**  |
| Mismo número de cada tipo en todos los servidores | **Obligatorio**  |
| Modelos de unidad misma en todos los servidores        | Recomendado   |
| Tamaños de unidad misma en todos los servidores         | Recomendado   |

## <a name="see-also"></a>Vea también

- [Requisitos de hardware de almacenamiento directo en espacios](storage-spaces-direct-hardware-requirements.md)
- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
