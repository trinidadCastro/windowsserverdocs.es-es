---
title: Consideraciones sobre la simetría de unidades para Espacios de almacenamiento directo
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2028d7b4ccb42d5da1426634541681f842c18972
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2020
ms.locfileid: "87768733"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>Consideraciones sobre la simetría de unidades para Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

[Espacios de almacenamiento directo](storage-spaces-direct-overview.md) funciona mejor cuando cada servidor tiene exactamente las mismas unidades.

En realidad, somos conscientes de que esto no siempre es práctico: Espacios de almacenamiento directo está diseñado para ejecutarse durante años y escalarse a medida que crezcan las necesidades de su organización. En la actualidad, puede comprar unidades de disco duro de 3 TB de "espacioso"; el año siguiente, puede ser imposible encontrar unos pequeños. Por lo tanto, se admite cierta cantidad de combinación y coincidencia.

En este tema se explican las restricciones y se proporcionan ejemplos de configuraciones admitidas y no admitidas.

## <a name="constraints"></a>Restricciones

### <a name="type"></a>Tipo

Todos los servidores deben tener los mismos [tipos de unidades](choosing-drives.md#drive-types).

Por ejemplo, si un servidor tiene NVMe, *todos* deben tener nVMe.

### <a name="number"></a>Number

Todos los servidores deben tener el mismo número de unidades de cada tipo.

Por ejemplo, si un servidor tiene seis SSD, *todos* deben tener seis SSD.

   > [!NOTE]
   > Es correcto que el número de unidades difieran temporalmente durante los errores o al agregar o quitar unidades.

### <a name="model"></a>Modelo

Siempre que sea posible, se recomienda usar unidades del mismo modelo y versión de firmware. Si no puede, seleccione cuidadosamente las unidades que sean lo más parecidas posible. Se desaconsejan las unidades de combinación y coincidencias del mismo tipo con características de rendimiento o resistencia muy diferentes (a menos que haya una caché y la otra sea de capacidad) porque Espacios de almacenamiento directo distribuye la e/s de forma uniforme y no discrimina según el modelo.

   > [!NOTE]
   > Es correcto mezclar y hacer coincidir unidades SATA y SAS similares.

### <a name="size"></a>Size

Se recomienda usar unidades del mismo tamaño siempre que sea posible. El uso de unidades de capacidad de distintos tamaños puede dar lugar a una capacidad inutilizable, y el uso de unidades de caché de distintos tamaños podría no mejorar el rendimiento de la memoria caché. Para obtener más información, vea la siguiente sección.

   > [!WARNING]
   > La diferencia entre los tamaños de unidades de capacidad de los servidores puede dar lugar a una capacidad por debajo del mismo.

## <a name="understand-capacity-imbalance"></a>Descripción: desequilibrio de capacidad

Espacios de almacenamiento directo es robusto para el desequilibrio de capacidad entre las unidades y entre servidores. Incluso si el desequilibrio es grave, todo seguirá funcionando. Sin embargo, en función de varios factores, es posible que no se pueda usar la capacidad que no está disponible en todos los servidores.

Para ver por qué ocurre esto, considere la ilustración simplificada a continuación. Cada cuadro coloreado representa una copia de los datos reflejados. Por ejemplo, los cuadros marcados como, ' y ' ' son tres copias de los mismos datos. Para respetar la tolerancia a errores del servidor, estas copias *deben* almacenarse en servidores diferentes.

### <a name="stranded-capacity"></a>Capacidad en desuso

Como se dibujó, el servidor 1 (10 TB) y el servidor 2 (10 TB) están llenos. El servidor 3 tiene unidades más grandes, por lo que su capacidad total es mayor (15 TB). Sin embargo, para almacenar más datos de reflejo triple en el servidor 3 también se necesitarían copias en el servidor 1 y el servidor 2, que ya están llenos. No se puede usar la capacidad de 5 TB restante en el servidor 3; su capacidad es *"en desuso"* .

![Reflejo triple, tres servidores, capacidad en desuso](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>Selección de ubicación óptima

Por el contrario, con cuatro servidores de 10 TB, 10 TB, 10 TB y capacidad de 15 TB y resistencia de reflejo triple, es posible colocar copias de manera válida de forma que utilice toda la capacidad disponible, tal como se *ha* dibujado. Siempre que esto sea posible, el asignador de Espacios de almacenamiento directo buscará y usará la ubicación óptima, sin dejar de tener capacidad en desuso.

![Reflejo triple, cuatro servidores, sin capacidad en desuso](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

El número de servidores, la resistencia, la gravedad del desequilibrio de capacidad y otros factores influyen en si hay capacidad en desuso. **La regla general más prudente es suponer que solo se puede usar la capacidad disponible en cada servidor.**

## <a name="understand-cache-imbalance"></a>Descripción: desequilibrio de caché

Espacios de almacenamiento directo es robusto para almacenar en caché el desequilibrio entre unidades y servidores. Incluso si el desequilibrio es grave, todo seguirá funcionando. Además, Espacios de almacenamiento directo siempre usa toda la memoria caché disponible al máximo.

Sin embargo, es posible que el uso de unidades de caché de distintos tamaños no mejore el rendimiento de la caché de forma uniforme o predecible: solo los [enlaces](understand-the-cache.md#server-side-architecture) de e/s a unidades de caché más grandes pueden ver un rendimiento mejorado. Espacios de almacenamiento directo distribuye la e/s uniformemente entre enlaces y no discrimina en función de la relación de caché a capacidad.

![Desequilibrio de caché](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Consulte [Descripción de la caché](understand-the-cache.md) para obtener más información sobre los enlaces de caché.

## <a name="example-configurations"></a>Configuraciones de ejemplo

Estas son algunas configuraciones admitidas y no admitidas:

### <a name="supported-supported-different-models-between-servers"></a>![admitido](media/drive-symmetry-considerations/supported.png) Compatible: diferentes modelos entre servidores

Los dos primeros servidores usan el modelo de NVMe "X", pero el tercer servidor usa el modelo de NVMe "Z", que es muy similar.

| Servidor 1                    | Servidor 2                    | Servidor 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x modelo de NVMe X (caché)    | 2 x modelo de NVMe X (caché)    | 2 x modelo de NVMe Z (caché)    |
| 10 modelo SSD Y (capacidad) | 10 modelo SSD Y (capacidad) | 10 modelo SSD Y (capacidad) |

Esta configuración es compatible.

### <a name="supported-supported-different-models-within-server"></a>![admitido](media/drive-symmetry-considerations/supported.png) Compatible: diferentes modelos en el servidor

Cada servidor utiliza una combinación diferente de modelos HDD "Y" y "Z", que son muy similares. Cada servidor tiene 10 unidades de disco duro en total.

| Servidor 1                   | Servidor 2                   | Servidor 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x modelo de SSD X (caché)    | 2 x modelo de SSD X (caché)    | 2 x modelo de SSD X (caché)    |
| 7 modelo de HDD Y (capacidad) | 5 modelo HDD Y (capacidad) | 1 x modelo de HDD Y (capacidad) |
| 3 x modelo HDD Z (capacidad) | 5 x modelo HDD Z (capacidad) | 9 x modelo HDD Z (capacidad) |

Esta configuración es compatible.

### <a name="supported-supported-different-sizes-across-servers"></a>![admitido](media/drive-symmetry-considerations/supported.png) Compatible: tamaños diferentes entre servidores

Los dos primeros servidores usan una unidad de disco duro de 4 TB, pero el tercer servidor usa una unidad de disco duro de 6 TB muy similar.

| Servidor 1                | Servidor 2                | Servidor 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (caché) | 2 x 800 GB NVMe (caché) | 2 x 800 GB NVMe (caché) |
| 4 unidades de disco duro de 4 TB (capacidad) | 4 unidades de disco duro de 4 TB (capacidad) | 4 unidades HDD de 6 TB (capacidad) |

Se admite, aunque se producirá una capacidad de perdedo.

### <a name="supported-supported-different-sizes-within-server"></a>![admitido](media/drive-symmetry-considerations/supported.png) Compatible: tamaños diferentes en el servidor

Cada servidor utiliza una combinación diferente de 1,2 TB y SSD de 1,6 TB muy similar. Cada servidor tiene 4 SSD en total.

| Servidor 1                 | Servidor 2                 | Servidor 3                 |
|--------------------------|--------------------------|--------------------------|
| SSD de 3 x 1,2 TB (caché)   | SSD de 2 x 1,2 TB (caché)   | SSD de 4 x 1,2 TB (caché)   |
| SSD de 1 x 1,6 TB (caché)   | SSD de 2 x 1,6 TB (caché)   | -                        |
| HDD de 20 x 4 TB (capacidad) | HDD de 20 x 4 TB (capacidad) | HDD de 20 x 4 TB (capacidad) |

Esta configuración es compatible.

### <a name="unsupported-not-supported-different-types-of-drives-across-servers"></a>![no admitido](media/drive-symmetry-considerations/unsupported.png) No compatible: diferentes tipos de unidades entre servidores

El servidor 1 tiene NVMe, pero los otros no.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (caché)    | -                   | -                   |
| -                   | SSD de 6 x (caché)     | SSD de 6 x (caché)     |
| 18 unidades HDD (capacidad) | 18 unidades HDD (capacidad) | 18 unidades HDD (capacidad) |

No es una opción admitida. Los tipos de unidades deben ser los mismos en todos los servidores.

### <a name="unsupported-not-supported-different-number-of-each-type-across-servers"></a>![no admitido](media/drive-symmetry-considerations/unsupported.png) No compatible: número diferente de cada tipo entre servidores

El servidor 3 tiene más unidades que las otras.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (caché)    | 2 x NVMe (caché)    | 4 x NVMe (caché)    |
| 10 unidades HDD (capacidad) | 10 unidades HDD (capacidad) | 20 unidades HDD (capacidad) |

No es una opción admitida. El número de unidades de cada tipo debe ser el mismo en todos los servidores.

### <a name="unsupported-not-supported-only-hdd-drives"></a>![no admitido](media/drive-symmetry-considerations/unsupported.png) No compatible: solo unidades HDD

Todos los servidores solo tienen unidades HDD conectadas.

|Servidor 1|Servidor 2|Servidor 3|
|-|-|-|
|18 unidades HDD (capacidad) |18 unidades HDD (capacidad)|18 unidades HDD (capacidad)|

No es una opción admitida. Debe agregar un mínimo de dos unidades de caché (NvME o SSD) conectadas a cada uno de los servidores.

## <a name="summary"></a>Resumen

En Resumen, cada servidor del clúster debe tener los mismos tipos de unidades y el mismo número de cada tipo. Se admite para combinar modelos de unidad y tamaños de unidad según sea necesario, con las consideraciones anteriores.

| Restricción | State |
|--|--|
| Los mismos tipos de unidades en cada servidor | **Obligatorio** |
| El mismo número de cada tipo en cada servidor | **Obligatorio** |
| Los mismos modelos de unidad en cada servidor | Recomendado |
| Los mismos tamaños de unidad en cada servidor | Recomendado |

## <a name="additional-references"></a>Referencias adicionales

- [Requisitos de hardware de Espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md)
- [Introducción a Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
