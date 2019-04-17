---
title: Consideraciones de simetría de unidad para espacios de almacenamiento directo
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: f2ef58003da6de049c7c4b578f789a97e0a0f512
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5591851"
---
# Consideraciones de simetría de unidad para espacios de almacenamiento directo 

> Se aplica a: Windows Server 2019, Windows Server 2016

[Espacios de almacenamiento directo](storage-spaces-direct-overview.md) funciona mejor cuando cada servidor tiene exactamente las mismas unidades.

En realidad, sabemos que esto no siempre resulta práctico: espacios de almacenamiento directo está diseñado para ejecutarse durante años y escalar a medida que crecen las necesidades de la organización. En la actualidad, puedes adquirir suficientes 3 TB unidades de disco duro; año siguiente, puede resultar imposible busca aquellas que pequeño. Por lo tanto, se admite cierta cantidad de mezcla y coincidentes.

En este tema se explica las restricciones y proporciona ejemplos de configuraciones compatibles y no compatibles.

## Restricciones

### Tipo

Todos los servidores deben tener los mismos [tipos de unidades](choosing-drives.md#drive-types).

Por ejemplo, si un servidor tiene NVMe, como deberían *todas* tienen NVMe.

### Número

Todos los servidores deben tener el mismo número de unidades de cada tipo.

Por ejemplo, si un servidor tiene seis SSD, como deberían *todas* tiene seis SSD.

   > [!NOTE]
   > Es aceptable para el número de unidades de para qué se diferencia temporalmente durante los errores o al agregar o quitar las unidades.

### Modelo

Te recomendamos que uses unidades del mismo modelo y la versión de firmware siempre que sea posible. Si no es posible, seleccionar con cuidado las unidades que sean lo más parecidas posible. Se desaconseja mezcla y coincidencia unidades del mismo tipo con características de rendimiento o resistencia entremezclan diferentes (a menos que uno es la memoria caché y el otro es capacidad), ya que distribuye uniformemente IO y no restrinja en el modelo de espacios de almacenamiento directo .

   > [!NOTE]
   > Es aceptable combinar y hacer coincidir unidades SATA y SAS similares.

### Tamaño

Te recomendamos que uses las unidades de los mismos tamaños de siempre que sea posible. Uso de unidades de capacidad de distintos tamaños puede producir algunos capacidad inutilizable y usar unidades de caché de distintos tamaños no puede mejorar el rendimiento de la caché. Consulta la siguiente sección para obtener más información.

   > [!WARNING]
   > Diferentes tamaños de unidades de capacidad en servidores pueden producir pérdida de capacidad.

## Comprender: desequilibrio de capacidad

Espacios de almacenamiento directo es eficaz desequilibrio capacidad a través de las unidades y servidores. Incluso si el desequilibrio es grave, todo lo seguirán funcionando. Sin embargo, dependiendo de varios factores, puede capacidad que no está disponible en todos los servidores de no ser utilizable.

Para ver por qué esto sucede, piensa en la siguiente ilustración simplificada. Cada cuadro coloreado representa una copia de datos de reflejadas. Por ejemplo, los cuadros de marcaron A, A' y un '' son tres copias de los mismos datos. Aceptar tolerancia a errores server, estas copias *deben* almacenarse en diferentes servidores.

### Pérdida de capacidad

Cuando se dibuja, servidor 1 (10 TB) y servidor 2 (10 TB) son completo. Servidor 3 tiene unidades más grandes, por lo tanto, su capacidad total es más grande (15 TB). Sin embargo almacenar datos de reflejo triple más en servidor 3 requeriría copias en Server 1 y 2 de servidor demasiado, que ya están completo. No se puede usar la capacidad de 5 TB restante en el servidor 3: es *"en desuso"* capacidad.

![Reflejo triple, tres servidores, capacidad en desuso](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### Ubicación óptima

Por el contrario, con cuatro servidores de 10 TB, 10 TB, 10 TB y 15 TB de capacidad y resistencia de reflejo triple, lo que *es* posible válida coloca copias de manera que toda la capacidad disponible, se usa como dibujado. Siempre que sea posible, el asignador de espacios de almacenamiento directo encontrará y usar la colocación óptima, no dejando ninguna pérdida de capacidad.

![Reflejo triple, cuatro servidores, sin pérdida de capacidad](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

El número de servidores, la resistencia, la gravedad de la desequilibrio capacidad y otros factores afectar si hay pérdida de capacidad. **La regla general más recomendable es asumir que solo capacidad disponible en todos los servidores se garantiza que se pueda usar.**

## Comprender: desequilibrio de caché

Espacios de almacenamiento directo es eficaz desequilibrio de caché a través de las unidades y servidores. Incluso si el desequilibrio es grave, todo lo seguirán funcionando. Además, espacios de almacenamiento directo siempre usa toda la memoria caché disponible al máximo.

Sin embargo, usando las unidades de caché de distintos tamaños puede no mejorar el rendimiento de la caché uniformemente o forma predecible: solo E/S a [los enlaces de unidad](understand-the-cache.md#server-side-architecture) con unidades de caché más grandes puede que veas mejorar el rendimiento. Espacios de almacenamiento directo distribuye IO uniformemente entre los enlaces y no distinguir en función de la relación de capacidad de caché.

![Desequilibrio de caché](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Vea la [Descripción de la memoria caché](understand-the-cache.md) para obtener más información sobre los enlaces de la memoria caché.

## Configuraciones de ejemplo

Estas son algunas configuraciones admitidas y no admitidas:

### ![admitido](media/drive-symmetry-considerations/supported.png) Admite: distintos modelos entre servidores

Los dos primeros servidores usan el modelo de NVMe "X", pero el tercer servidor usa el modelo de NVMe "Z", que es muy similar.

| Servidor 1                    | Servidor 2                    | Servidor 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe modelo X (caché)    | 2 x NVMe modelo X (caché)    | 2 x Z del modelo de NVMe (memoria caché)    |
| 10 x SSD modelo Y (capacidad) | 10 x SSD modelo Y (capacidad) | 10 x SSD modelo Y (capacidad) |

Esto se admite.

### ![admitido](media/drive-symmetry-considerations/supported.png) Admite: distintos modelos dentro de servidor

Cada servidor usa algunas mezcla diferentes de modelos de unidad de disco duro "Y" y "Z", que son muy similares. Cada servidor tiene 10 HDD total.

| Servidor 1                   | Servidor 2                   | Servidor 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD modelo X (caché)    | 2 x SSD modelo X (caché)    | 2 x SSD modelo X (caché)    |
| 7 x Y del modelo de unidad de disco duro (capacidad) | 5 x Y del modelo de unidad de disco duro (capacidad) | 1 x Y del modelo de unidad de disco duro (capacidad) |
| 3 x Z del modelo de unidad de disco duro (capacidad) | 5 x Z del modelo de unidad de disco duro (capacidad) | 9 x Z del modelo de unidad de disco duro (capacidad) |

Esto se admite.

### ![admitido](media/drive-symmetry-considerations/supported.png) Admite: tamaños diferentes a través de servidores

Los dos primeros servidores usan 4 TB HDD pero el tercer servidor usa muy similar 6 TB HDD.

| Servidor 1                | Servidor 2                | Servidor 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (memoria caché) | 2 x 800 GB NVMe (memoria caché) | 2 x 800 GB NVMe (memoria caché) |
| 4 x 4 TB HDD (capacidad) | 4 x 4 TB HDD (capacidad) | 4 x 6 TB HDD (capacidad) |

Esto se admite, aunque resultará en desuso capacidad.

### ![admitido](media/drive-symmetry-considerations/supported.png) Admite: distintos tamaños dentro de servidor

Cada servidor usa algunas mezcla diferentes de TB 1.2 y muy similar 1,6 TB SSD. Cada servidor tiene 4 SSD total.

| Servidor 1                 | Servidor 2                 | Servidor 3                 |
|--------------------------|--------------------------|--------------------------|
| 1.2 x 3 TB SSD (caché)   | 2 x 1.2 TB SSD (caché)   | 1.2 x 4 TB SSD (caché)   |
| 1 x 1,6 TB SSD (caché)   | 2 x 1,6 TB SSD (caché)   | -                        |
| 20 x 4 TB HDD (capacidad) | 20 x 4 TB HDD (capacidad) | 20 x 4 TB HDD (capacidad) |

Esto se admite.

### ![no admitido](media/drive-symmetry-considerations/unsupported.png) No compatible: diferentes tipos de unidades a través de servidores

1 de servidor tiene NVMe, pero los demás no.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (caché)    | -                   | -                   |
| -                   | 6 x SSD (caché)     | 6 x SSD (caché)     |
| 18 x HDD (capacidad) | 18 x HDD (capacidad) | 18 x HDD (capacidad) |

Esto no es compatible. Los tipos de unidades deben ser el mismo en cada servidor.

### ![no admitido](media/drive-symmetry-considerations/unsupported.png) No compatible: número diferente de cada tipo en servidores

Servidor 3 tiene más unidades que las demás.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (caché)    | 2 x NVMe (caché)    | 4 x NVMe (caché)    |
| 10 x HDD (capacidad) | 10 x HDD (capacidad) | 20 x HDD (capacidad) |

Esto no es compatible. El número de unidades de cada tipo de debe ser el mismo en cada servidor.

### ![no admitido](media/drive-symmetry-considerations/unsupported.png) No compatible: solo las unidades de disco duro

Todos los servidores tienen solo las unidades de disco duro conectadas.

|Servidor 1|Servidor 2|Servidor 3|
|-|-|-| 
|18 x HDD (capacidad) |18 x HDD (capacidad)|18 x HDD (capacidad)|

Esto no es compatible. Debes agregar un mínimo de dos unidades de caché (NvME o SSD) asociado a cada uno de los servidores.

## Resumen

En resumen, todos los servidores del clúster deben tener los mismos tipos de unidades y el mismo número de cada tipo. Se admite a los modelos de unidad de combinar y hacer coincidir y tamaños de unidad según sea necesario, con las consideraciones anteriores.

| Restricción                               |               |
|------------------------------------------|---------------|
| Mismos tipos de unidades en cada servidor     | **Obligatorio**  |
| Mismo número de cada tipo en cada servidor | **Obligatorio**  |
| Misma modelos de unidad en cada servidor        | Recomendaciones   |
| Mismos tamaños de unidad en cada servidor         | Recomendaciones   |

## Ver también

- [Requisitos de hardware de Espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md)
- [Información general de Espacios de almacenamiento directos](storage-spaces-direct-overview.md)
