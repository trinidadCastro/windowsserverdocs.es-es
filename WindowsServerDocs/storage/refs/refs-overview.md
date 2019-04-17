---
title: "Información general del Sistema de archivos resistente (ReFS)"
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: dmoss
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 1/3/2016
ms.assetid: 
ms.openlocfilehash: 4460df978f2a3d5e30e43b82cf952ed37f3d91a9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="resilient-file-system-refs-overview"></a>Información general del Sistema de archivos resistente (ReFS)
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El Sistema de archivos resistente (ReFS) es el nuevo sistema de archivos de Microsoft, que está diseñado para maximizar la disponibilidad de los datos, escalar de manera eficiente a conjuntos de datos de gran tamaño en diversas cargas de trabajo y proporciona la integridad de los datos por medio de la resistencia a los daños. Intenta solucionar un creciente conjunto de escenarios de almacenamiento y establecer una base para futuras innovaciones. 

## <a name="key-benefits"></a>Principales ventajas

### <a name="resiliency"></a>Resistencia 
ReFS presenta nuevas características que pueden detectar daños con precisión y también solucionarlos mientras aún está en línea, que te ayudarán a proporcionar una mayor integridad y disponibilidad para tus datos: 

- **Secuencias de integridad** -ReFS usa sumas de comprobación para los metadatos y, opcionalmente, para datos de archivos, lo que da ReFS la capacidad de detectar daños de manera confiable. 
- **Integración de espacios de almacenamiento**: Cuando se usa junto a un espacio de reflejo o de paridad, ReFS puede reparar automáticamente daños detectados usando la copia alternativa de los datos proporcionados por los espacios de almacenamiento. Los procesos de reparación se localizan en el área del daño y se ejecuta en línea, por lo que no se requiere tiempo de inactividad de los volúmenes.
- **Recuperación de datos**: Si se produce algún tipo de daño en un volumen y no existe una versión alternativa de los datos dañados, ReFS elimina los datos dañados del espacio de nombres. ReFS mantiene el volumen en línea mientras se ocupa de la mayoría de los daños que no pueden corregirse, pero existen algunos casos excepcionales en los que ReFS tiene que ocuparse de este volumen en modo sin conexión.
- **Corrección de errores proactiva**: Además de validar datos antes de las lecturas y escrituras, ReFS presenta un escáner de integridad de datos, conocido como <i>limpieza</i>. Esta limpieza escanea el volumen de manera periódica, identificando los daños latentes y desencadenando de manera proactiva una reparación de los datos dañados. 


### <a name="performance"></a>Rendimiento
Además de proporcionar mejoras de resistencia, ReFS presenta nuevas características para cargas de trabajo dependientes del rendimiento y virtualizadas. La optimización de capa en tiempo real, la clonación de bloque y el VDL disperso son buenos ejemplos de las capacidades evolutivas de ReFS, que está diseñadas para ser compatibles con cargas de trabajo dinámicas, entre otras.

- **[Paridad acelerada por reflejos](./mirror-accelerated-parity.md)**: la paridad acelerada por reflejos ofrece alto rendimiento, así como una capacidad de almacenamiento eficiente para los datos. 

    - Para ofrecer alto rendimiento y capacidad de almacenamiento eficiente, ReFS divide un volumen en dos grupos de almacenamiento lógicos, denominados capas. Estas capas pueden tener sus propios tipos de unidad y resistencia, lo que permite a cada capa una optimización ya sea del rendimiento o la capacidad. Algunas configuraciones de ejemplo incluyen: 
    
    
    | Capa de rendimiento | Capacidad de nivel |
    |----------------|-----------------|
     SSD de espejo | HDD de espejo |
     SSD de espejo | SSD de paridad |
     SSD de espejo | HDD de paridad |   
            
    - Cuando estas capas están configuradas, ReFS las usa para ofrecer un almacenamiento rápido para los datos "en caliente" (usados con mucha frecuencia) y la capacidad de almacenamiento eficiente para los datos "fríos" (aquellos que se escriben con poca frecuencia).
        - Todas las escrituras ocurren en la capa de rendimiento y las grandes cantidades de datos que permanecen en el nivel de rendimiento se moverán de manera eficaz al nivel de capacidad en tiempo real.
        - Si utilizas una [implementación híbrida](../storage-spaces/choosing-drives.md) (que mezcla flash y unidades de disco duro), [la memoria caché en los Espacios de almacenamiento directo](../storage-spaces/understand-the-cache.md) acelerará el proceso de las lecturas, reduciendo así el efecto de la fragmentación datos, característico de las cargas de trabajo virtualizadas. De lo contrario, si usas una implementación de flash completa, las lecturas también se producirán en la capa de rendimiento. 

>[!NOTE]
>Para las implementaciones de servidor, la paridad acelerada por reflejos solo es compatible con [Espacios de almacenamiento directos](../storage-spaces/storage-spaces-direct-overview.md).

- **Operaciones de VM con aceleración**: ReFS presenta nuevas funcionalidades especialmente dirigidas a mejorar el rendimiento de las cargas de trabajo virtualizadas:
    - [Bloque de clonación](./block-cloning.md): El bloque de clonación acelera las operaciones de copia, habilitando rápidas operaciones de fusión de punto de control con una VM de bajo impacto. 
    - VDL disperso - El VDL disperso permite a ReFS eliminar los archivos rápidamente, reduciendo el tiempo necesario para crear VHD solucionados de 10 segundos de minutos a tan solo unos segundos.


- **Tamaños variables de clúster** -ReFS admite tamaños de clúster de entre 4K y 64 KB. 4K es el tamaño de clúster recomendado para la mayoría de las implementaciones, pero los clústeres de 64 KB son adecuados para las cargas de trabajo de E/S de gran tamaño y secuenciales.
    
    
### **<a name="scalability"></a>Escalabilidad**
ReFS está diseñado para admitir inmensos grupos de datos, millones de terabytes, sin que esto tenga un impacto negativo en el rendimiento, consiguiendo así una mayor escala que los sistemas de archivos anteriores. 

## <a name="supported-deployments"></a>Implementaciones admitidas

### <a name="storage-spaces-direct"></a>Espacios de almacenamiento directo ###

La implementación de ReFS en los Espacios de almacenamiento directo es la configuración recomendada para las cargas de trabajo virtualizadas o el almacenamiento conectado a la red: 
- La paridad acelerada por reflejos y [la memoria caché en los Espacios de almacenamiento directo](../storage-spaces/understand-the-cache.md) ofrecen un alto rendimiento y una capacidad de almacenamiento eficiente. 
- La introducción de la clonación de bloques y el VDL disperso acelera en gran medida las operaciones del archivo .vhdx, como la creación, combinación y expansión.
- Las secuencias de integridad, las reparaciones en línea y las copias de datos alternativos permite a ReFS y a los Espacios de almacenamiento directo detectar y corregir los daños de los datos y metadatos. 
- ReFS proporciona la funcionalidad para escalar y ser compatible con grandes grupos de datos. 

### <a name="storage-spaces-with-sas-drive-enclosures"></a>Espacios de almacenamiento con contenedores de unidad SAS ###
La implementación de ReFS en los espacios de almacenamiento con contenedores de unidad SAS compartidos es la apropiada para alojar el archivo de datos y almacenar los documentos de los usuarios:
- Las secuencias de integridad, las reparaciones en línea y las copias de datos alternativos permite a ReFS y a los Espacios de almacenamiento detectar y corregir los daños de los datos y metadatos. 
- Las implementaciones de espacios de almacenamiento también pueden usar clonación de bloques y la escalabilidad que se ofrecen en ReFS.

### <a name="basic-disks"></a>Discos básicos ###
Implementar ReFS en los discos básicos es la mejor opción para las aplicaciones que implementan sus propias soluciones de resistencia y disponibilidad de software. 
- Las aplicaciones que ofrecen sus propias soluciones de software de resistencia y disponibilidad pueden sacar partido de las secuencias de integridad, la clonación de bloques y la capacidad de escalar y ser compatible con grandes grupos de datos. 

>[!NOTE]
>ReFS no es compatible con el almacenamiento conectado a red SAN.


## <a name="feature-comparison"></a>Comparación de características

### <a name="limits"></a>Límites

| Función       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Longitud máxima del nombre del archivo | 255 caracteres Unicode  | 255 caracteres Unicode               |
| Longitud máxima del nombre de la ruta |32 000 caracteres Unicode | 32 000 caracteres Unicode                |
| Tamaño máximo del archivo | 18 EB (exabytes)  | 18 EB (exabytes)                |
| Tamaño máximo del volumen | 4,7 ZB (zettabytes)                           | 256TB                |


### <a name="functionality"></a>Funcionalidad

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>Las siguientes características están disponibles en ReFS y NTFS:

| Funcionalidad       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Cifrado de BitLocker | Sí | Sí |
| Compatibilidad con volumen compartido en clúster (CSV) | Sí | Sí |
| Enlaces simbólicos | Sí | Sí |
| Compatibilidad con clústeres de conmutación por error | Sí | Sí |
| Listas de control de acceso | Sí | Sí |
| Diario de USN | Sí | Sí |
| Notificación de cambios | Sí | Sí |
| Puntos de unión | Sí | Sí |
| Puntos de montaje | Sí | Sí |
| Puntos de reanálisis | Sí | Sí |
| Instantáneas de volumen | Sí | Sí |
| Id. de archivo | Sí | Sí |
| Bloqueos oportunistas | Sí | Sí |
| Archivos dispersos | Sí | Sí |
| Secuencias con nombre | Sí | Sí |

#### <a name="the-following-features-are-only-available-on-refs"></a>Las siguientes características solo están disponibles en ReFS:

| Funcionalidad       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Clonación de bloques | Sí | No |
| VDL disperso | Sí | No |
| Paridad acelerada por reflejos| Sí (en los Espacios de almacenamiento directo) | No |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>Las siguientes características no están disponibles en ReFS en este momento:

| Funcionalidad       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Compresión de sistema de archivos | No | Sí |
| Cifrado de sistema de archivos | No | Sí |
| Desduplicación de datos | No | Sí |
| Transacciones | No | Sí |
| Vínculos físicos | No | Sí |
| Id. de objeto | No | Sí |
| Nombres cortos | No | Sí |
| Atributos extendidos | No | Sí |
| Cuotas de disco | No | Sí |
| Ejecutable | No | Sí |
| Compatible con medios extraíbles | No | Sí |
| Capas de almacenamiento de NTFS | No | Sí |


## <a name="see-also"></a>Consulta también

-   [Clonación de bloques de ReFS](block-cloning.md)
-   [Secuencias de integridad de ReFS](integrity-streams.md)
-   [Información general de Espacios de almacenamiento directos](../storage-spaces/storage-spaces-direct-overview.md)
