---
title: Información general del Sistema de archivos resistente (ReFS)
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: mchad
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 10/17/2018
ms.openlocfilehash: 75f13a715baa0c0943e1521662d6d318b19e075b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869756"
---
# <a name="resilient-file-system-refs-overview"></a>Información general del Sistema de archivos resistente (ReFS)

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semianual)

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

- **[Paridad acelerada reflejado](./mirror-accelerated-parity.md)**  -acelerada reflejado paridad ofrece alto rendimiento y también capacidad eficaz de almacenamiento para los datos. 

    - Para ofrecer alto rendimiento y capacidad de almacenamiento eficiente, ReFS divide un volumen en dos grupos de almacenamiento lógicos, denominados capas. Estas capas pueden tener sus propios tipos de unidad y resistencia, lo que permite a cada capa una optimización ya sea del rendimiento o la capacidad. Algunas configuraciones de ejemplo incluyen: 
    
      | Capa de rendimiento | Capacidad de nivel |
      |----------------|-----------------|
       SSD de espejo | HDD de espejo |
       SSD de espejo | SSD de paridad |
       SSD de espejo | HDD de paridad |
            
    - Cuando estas capas están configuradas, ReFS las usa para ofrecer un almacenamiento rápido para los datos "en caliente" (usados con mucha frecuencia) y la capacidad de almacenamiento eficiente para los datos "fríos" (aquellos que se escriben con poca frecuencia).
        - Todas las escrituras ocurren en la capa de rendimiento y las grandes cantidades de datos que permanecen en el nivel de rendimiento se moverán de manera eficaz al nivel de capacidad en tiempo real.
        - Si usa una implementación híbrida (combinación de flash y unidades de disco duro), [la memoria caché en espacios de almacenamiento directo](../storage-spaces/understand-the-cache.md) ayuda a acelerar las lecturas, lo que reduce el efecto de los datos fragmentación característica de virtualiza las cargas de trabajo. En caso contrario, si usa una implementación de memoria flash, lee también se producen en el nivel de rendimiento.

>[!NOTE]
>Para las implementaciones de servidor, la paridad acelerada por reflejos solo es compatible con [Espacios de almacenamiento directos](../storage-spaces/storage-spaces-direct-overview.md). Se recomienda usar la paridad reflejado acelerada con archivado y copia de seguridad las cargas de trabajo solo. Para cargas de trabajo aleatorias virtualizado y otras de alto rendimiento, se recomienda el uso de reflejos de tres vías para mejorar el rendimiento.

- **Operaciones de VM con aceleración**: ReFS presenta nuevas funcionalidades especialmente dirigidas a mejorar el rendimiento de las cargas de trabajo virtualizadas:
    - [Bloque de clonación](./block-cloning.md): El bloque de clonación acelera las operaciones de copia, habilitando rápidas operaciones de fusión de punto de control con una VM de bajo impacto.
    - VDL disperso - El VDL disperso permite a ReFS eliminar los archivos rápidamente, reduciendo el tiempo necesario para crear VHD solucionados de 10 segundos de minutos a tan solo unos segundos.


- **Tamaños variables de clúster** -ReFS admite tamaños de clúster de entre 4K y 64 KB. 4K es el tamaño de clúster recomendado para la mayoría de las implementaciones, pero los clústeres de 64 KB son adecuados para las cargas de trabajo de E/S de gran tamaño y secuenciales.
    
    
### <a name="scalability"></a>Escalabilidad
ReFS está diseñado para admitir inmensos grupos de datos, millones de terabytes, sin que esto tenga un impacto negativo en el rendimiento, consiguiendo así una mayor escala que los sistemas de archivos anteriores. 

## <a name="supported-deployments"></a>Implementaciones admitidas

Microsoft ha desarrollado NTFS específicamente para el uso general con una amplia gama de configuraciones y las cargas de trabajo, sin embargo, para los clientes especialmente que requieren la disponibilidad, resistencia y escala que proporciona ReFS, Microsoft admite a ReFS para su uso en las siguientes configuraciones y escenarios. 

>[!NOTE]
> Deben usar todas las configuraciones de ReFS admitidas [Windows Server Catalog](https://www.WindowsServerCatalog.com) certified requisitos de hardware y a cumplir con la aplicación.

### <a name="storage-spaces-direct"></a>Espacios de almacenamiento directo

La implementación de ReFS en los Espacios de almacenamiento directo es la configuración recomendada para las cargas de trabajo virtualizadas o el almacenamiento conectado a la red: 
- La paridad acelerada por reflejos y [la memoria caché en los Espacios de almacenamiento directo](../storage-spaces/understand-the-cache.md) ofrecen un alto rendimiento y una capacidad de almacenamiento eficiente. 
- La introducción de la clonación de bloques y el VDL disperso acelera en gran medida las operaciones del archivo .vhdx, como la creación, combinación y expansión.
- Secuencias de integridad, reparación en línea y copias de datos alternativas permiten a ReFS y espacios de almacenamiento directo para conjuntamente para detectar y corregir los daños de medios de almacenamiento dentro de los metadatos y datos y la controladora de almacenamiento. 
- ReFS proporciona la funcionalidad para escalar y ser compatible con grandes grupos de datos. 

### <a name="storage-spaces"></a>Espacios de almacenamiento

- Secuencias de integridad, reparación en línea y copias de datos alternativas permiten ReFS y [espacios de almacenamiento](../storage-spaces/overview.md) a conjuntamente para detectar y corregir los daños de medios de almacenamiento dentro de los metadatos y datos y la controladora de almacenamiento.
- Las implementaciones de espacios de almacenamiento también pueden usar clonación de bloques y la escalabilidad que se ofrecen en ReFS.
- Implementación de ReFS en espacios de almacenamiento con contenedores SAS compartidos es adecuado para hospedar los datos de archivo y almacenar documentos de usuario.

>[!NOTE]
> Almacenamiento espacios admite local no extraíble conectado directamente a través de BusTypes SATA, SAS, NVME o conectados a través de HBA (también conocido como controlador RAID en modo paso a través).

### <a name="basic-disks"></a>Discos básicos

Implementación de ReFS en discos básicos es ideal para las aplicaciones que implementan sus propias soluciones de resistencia y disponibilidad de software. 
- Las aplicaciones que ofrecen sus propias soluciones de software de resistencia y disponibilidad pueden sacar partido de las secuencias de integridad, la clonación de bloques y la capacidad de escalar y ser compatible con grandes grupos de datos. 


>[!NOTE]
> Los discos básicos incluyen a local no extraíble conectado directamente a través de BusTypes SATA, SAS, NVME o RAID. 

### <a name="backup-target"></a>Destino de copia de seguridad

Implementación de ReFS como un destino de copia de seguridad es mejor adecuados para las aplicaciones y hardware que implementan sus propias soluciones de resistencia y disponibilidad.
- Las aplicaciones que ofrecen sus propias soluciones de software de resistencia y disponibilidad pueden sacar partido de las secuencias de integridad, la clonación de bloques y la capacidad de escalar y ser compatible con grandes grupos de datos.

>[!NOTE]
> Destinos de copia de seguridad incluyen las configuraciones admitidas mencionadas anteriormente. Póngase en contacto con los proveedores de matrices de almacenamiento y la aplicación para obtener más información de soporte técnico en SAN iSCSI y de canal de fibra. Para redes SAN, si se requieren, características como el aprovisionamiento fino, TRIM/UNMAP o transferencia de datos descargados (ODX) NTFS se debe usar.   

## <a name="feature-comparison"></a>Comparación de características

### <a name="limits"></a>Límites

| Característica       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Longitud máxima del nombre del archivo | 255 caracteres Unicode  | 255 caracteres Unicode               |
| Longitud máxima del nombre de la ruta |32 000 caracteres Unicode | 32 000 caracteres Unicode                |
| Tamaño máximo del archivo | 35 PB (petabytes)  | 256 TB               |
| Tamaño máximo del volumen | 35 PB                           | 256 TB                |

### <a name="functionality"></a>Funcionalidad

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>Las siguientes características están disponibles en ReFS y NTFS:

| Funcionalidad       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Cifrado de BitLocker | Sí | Sí |
| Desduplicación de datos | Sí<sup>1</sup> | Sí |
| Compatibilidad con volumen compartido en clúster (CSV) | Sí<sup>2</sup> | Sí |
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
| Aprovisionamiento fino | Sí<sup>3</sup> | Sí |
| Transferencia de datos descargados (ODX) | No | Sí |
| Trim y Unmap | Sí<sup>3</sup> | Sí |
1. Está disponible en Windows Server, versión 1709 y versiones posterior.
2. Disponible en Windows Server 2012 R2 y versiones posteriores.
3. Solo los espacios de almacenamiento

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
| Transacciones | No | Sí |
| Vínculos físicos | No | Sí |
| Id. de objeto | No | Sí |
| Nombres cortos | No | Sí |
| Atributos extendidos | No | Sí |
| Cuotas de disco | No | Sí |
| Ejecutable | No | Sí |
| Compatibilidad con archivos de página | No | Sí |
| Compatible con medios extraíbles | No | Sí |


## <a name="see-also"></a>Vea también

-   [Información general de espacios directo de almacenamiento](../storage-spaces/storage-spaces-direct-overview.md)
-   [Clonación de bloques de reFS](block-cloning.md)
-   [Secuencias de integridad de reFS](integrity-streams.md)

