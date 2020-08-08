---
title: Información general del Sistema de archivos resistente (ReFS)
ms.author: gawatu
manager: mchad
ms.topic: article
author: gawatu
ms.date: 06/29/2019
ms.openlocfilehash: 668ee7a0c9e948c12140d3e25309a68ad3b2148b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957272"
---
# <a name="resilient-file-system-refs-overview"></a>Información general del Sistema de archivos resistente (ReFS)

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semianual)

El Sistema de archivos resistente (ReFS) es el nuevo sistema de archivos de Microsoft, que está diseñado para maximizar la disponibilidad de los datos, escalar de manera eficiente a conjuntos de datos de gran tamaño en diversas cargas de trabajo y proporciona la integridad de los datos por medio de la resistencia a los daños. Intenta solucionar un creciente conjunto de escenarios de almacenamiento y establecer una base para futuras innovaciones.

## <a name="key-benefits"></a>Ventajas principales

### <a name="resiliency"></a>Resistencia

ReFS presenta nuevas características que pueden detectar daños con precisión y también solucionarlos mientras aún está en línea, que te ayudarán a proporcionar una mayor integridad y disponibilidad para tus datos:

- **Secuencias de integridad** -ReFS usa sumas de comprobación para los metadatos y, opcionalmente, para datos de archivos, lo que da ReFS la capacidad de detectar daños de manera confiable.
- **Integración de espacios de almacenamiento**: Cuando se usa junto a un espacio de reflejo o de paridad, ReFS puede reparar automáticamente daños detectados usando la copia alternativa de los datos proporcionados por los espacios de almacenamiento. Los procesos de reparación se localizan en el área del daño y se ejecuta en línea, por lo que no se requiere tiempo de inactividad de los volúmenes.
- **Recuperación de datos**: Si se produce algún tipo de daño en un volumen y no existe una versión alternativa de los datos dañados, ReFS elimina los datos dañados del espacio de nombres. ReFS mantiene el volumen en línea mientras se ocupa de la mayoría de los daños que no pueden corregirse, pero existen algunos casos excepcionales en los que ReFS tiene que ocuparse de este volumen en modo sin conexión.
- **Corrección de errores proactiva**: Además de validar datos antes de las lecturas y escrituras, ReFS presenta un escáner de integridad de datos, conocido como <i>limpieza</i>. Esta limpieza escanea el volumen de manera periódica, identificando los daños latentes y desencadenando de manera proactiva una reparación de los datos dañados.

### <a name="performance"></a>Rendimiento

Además de proporcionar mejoras de resistencia, ReFS presenta nuevas características para cargas de trabajo dependientes del rendimiento y virtualizadas. La optimización de capa en tiempo real, la clonación de bloque y el VDL disperso son buenos ejemplos de las capacidades evolutivas de ReFS, que está diseñadas para ser compatibles con cargas de trabajo dinámicas, entre otras.

- **[Paridad acelerada para reflejo](./mirror-accelerated-parity.md)** : la paridad acelerada de reflejo ofrece un alto rendimiento y un almacenamiento eficaz para sus datos.

    - Para ofrecer almacenamiento eficaz y de alto rendimiento, ReFS divide un volumen en dos grupos de almacenamiento lógicos, conocidos como niveles. Estas capas pueden tener sus propios tipos de unidad y resistencia, lo que permite a cada capa una optimización ya sea del rendimiento o la capacidad. Algunas configuraciones de ejemplo incluyen:

      | Nivel de rendimiento | Capacidad de nivel |
      | ---------------- | ----------------- |
      | SSD de espejo | HDD de espejo |
      | SSD de espejo | SSD de paridad |
      | SSD de espejo | HDD de paridad |

    - Cuando estas capas están configuradas, ReFS las usa para ofrecer un almacenamiento rápido para los datos "en caliente" (usados con mucha frecuencia) y la capacidad de almacenamiento eficiente para los datos "fríos" (aquellos que se escriben con poca frecuencia).
        - Todas las escrituras ocurren en la capa de rendimiento y las grandes cantidades de datos que permanecen en el nivel de rendimiento se moverán de manera eficaz al nivel de capacidad en tiempo real.
        - Si usa una implementación híbrida (que combina unidades Flash y HDD), [la memoria caché de espacios de almacenamiento directo](../storage-spaces/understand-the-cache.md) ayuda a acelerar las lecturas, lo que reduce el efecto de la característica de fragmentación de datos de cargas de trabajo virtualizadas. De lo contrario, si se usa una implementación de All-Flash, las lecturas también se producen en el nivel de rendimiento.

> [!NOTE]
> En el caso de las implementaciones de servidor, la paridad con aceleración de reflejo solo se admite en [espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md). Se recomienda usar la paridad acelerada para reflejo solo con cargas de trabajo de archivado y copia de seguridad. En el caso de cargas de trabajo aleatorias virtualizadas y otras de alto rendimiento, se recomienda utilizar reflejos triples para mejorar el rendimiento.

- **Operaciones de VM con aceleración**: ReFS presenta nuevas funcionalidades especialmente dirigidas a mejorar el rendimiento de las cargas de trabajo virtualizadas:
    - [Bloque de clonación](./block-cloning.md): El bloque de clonación acelera las operaciones de copia, habilitando rápidas operaciones de fusión de punto de control con una VM de bajo impacto.
    - VDL disperso - El VDL disperso permite a ReFS eliminar los archivos rápidamente, reduciendo el tiempo necesario para crear VHD solucionados de 10 segundos de minutos a tan solo unos segundos.

- **Tamaños variables de clúster** -ReFS admite tamaños de clúster de entre 4K y 64 KB. 4K es el tamaño de clúster recomendado para la mayoría de las implementaciones, pero los clústeres de 64 KB son adecuados para las cargas de trabajo de E/S de gran tamaño y secuenciales.

### <a name="scalability"></a>Escalabilidad

ReFS está diseñado para admitir inmensos grupos de datos, millones de terabytes, sin que esto tenga un impacto negativo en el rendimiento, consiguiendo así una mayor escala que los sistemas de archivos anteriores.

## <a name="supported-deployments"></a>Implementaciones admitidas

Microsoft ha desarrollado NTFS específicamente para uso general con una amplia gama de configuraciones y cargas de trabajo, pero para clientes que requieren especial la disponibilidad, la resistencia o la escala que proporciona ReFS, Microsoft admite ReFS para su uso en los siguientes escenarios y configuraciones.

> [!NOTE]
> Todas las configuraciones compatibles con ReFS deben usar hardware certificado con el [Catálogo de Windows Server](https://www.WindowsServerCatalog.com) y cumplir los requisitos de la aplicación.

### <a name="storage-spaces-direct"></a>Espacios de almacenamiento directos

Se recomienda implementar ReFS en Espacios de almacenamiento directo para cargas de trabajo virtualizadas o almacenamiento conectado a la red:
- La paridad con aceleración de reflejo y [la memoria caché en espacios de almacenamiento directo](../storage-spaces/understand-the-cache.md) proporcionan un almacenamiento de alto rendimiento y con una capacidad eficaz.
- La introducción de la clonación de bloques y el VDL disperso acelera en gran medida las operaciones del archivo .vhdx, como la creación, combinación y expansión.
- La integridad: las secuencias, la reparación en línea y las copias de datos alternativas permiten a ReFS y Espacios de almacenamiento directo conjuntamente detectar y corregir los daños en los medios de almacenamiento y el controlador de almacenamiento en los metadatos y los datos.
- ReFS proporciona la funcionalidad para escalar y admitir grandes conjuntos de datos.

### <a name="storage-spaces"></a>Espacios de almacenamiento

- Integridad: las secuencias, la reparación en línea y las copias de datos alternativas permiten que las referencias y los [espacios de almacenamiento](../storage-spaces/overview.md) permitan de forma conjunta detectar y corregir los daños en los medios de almacenamiento y el controlador de almacenamiento en los metadatos y los datos.
- Las implementaciones de espacios de almacenamiento también pueden emplear la clonación de bloque y la escalabilidad que se ofrece en ReFS.
- La implementación de ReFS en espacios de almacenamiento con contenedores de SAS compartidos es adecuada para hospedar datos de archivo y almacenar documentos de usuario.

> [!NOTE]
> Los espacios de almacenamiento admiten conexión directa no extraíble local a través de BusTypes SATA, SAS, NVME o conectado a través de HBA (también conocido como controlador RAID en modo de paso a través).

### <a name="basic-disks"></a>Discos básicos

La implementación de ReFS en discos básicos es más adecuada para las aplicaciones que implementan sus propias soluciones de disponibilidad y resistencia de software.
- Las aplicaciones que presentan sus propias soluciones de software de disponibilidad y resistencia pueden aprovechar las secuencias de integridad, la clonación de bloques y la capacidad de escalar y admitir grandes conjuntos de datos.

> [!NOTE]
> Los discos básicos incluyen conexión directa no extraíble local a través de BusTypes SATA, SAS, NVME o RAID. Los discos básicos no incluyen espacios de almacenamiento.

### <a name="backup-target"></a>Destino de copia de seguridad

La implementación de ReFS como destino de copia de seguridad es la más adecuada para las aplicaciones y el hardware que implementan sus propias soluciones de resistencia y disponibilidad.
- Las aplicaciones que presentan sus propias soluciones de software de disponibilidad y resistencia pueden aprovechar las secuencias de integridad, la clonación de bloques y la capacidad de escalar y admitir grandes conjuntos de datos.

> [!NOTE]
> Los destinos de copia de seguridad incluyen las configuraciones admitidas anteriormente. Póngase en contacto con proveedores de aplicaciones y de matrices de almacenamiento para obtener información detallada sobre el canal de fibra y las SAN iSCSI. En el caso de las redes SAN, si se requieren características como el aprovisionamiento fino, el recorte o la desasignación o el Transferencia de datos descargado (ODX), se debe usar NTFS.

## <a name="feature-comparison"></a>Comparación de características

### <a name="limits"></a>límites

| Característica       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Longitud máxima del nombre del archivo | 255 caracteres Unicode  | 255 caracteres Unicode               |
| Longitud máxima del nombre de la ruta |32 000 caracteres Unicode | 32 000 caracteres Unicode                |
| Tamaño de archivo máximo | 35 PB (petabytes)  | 256 TB               |
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
| Diario USN | Sí | Sí |
| Notificación de cambios | Sí | Sí |
| Puntos de unión | Sí | Sí |
| Puntos de montaje | Sí | Sí |
| Puntos de repetición de análisis | Sí | Sí |
| Instantáneas de volumen | Sí | Sí |
| Id. de archivo | Sí | Sí |
| Oplocks | Sí | Sí |
| Archivos dispersos | Sí | Sí |
| Secuencias con nombre | Sí | Sí |
| Aprovisionamiento fino | Sí<sup>3</sup> | Sí |
| Recortar/desasignar | Sí<sup>3</sup> | Sí |
1. Disponible en Windows Server, versión 1709 y versiones posteriores.
2. Disponible en Windows Server 2012 R2 y versiones posteriores.
3. Solo espacios de almacenamiento

#### <a name="the-following-features-are-only-available-on-refs"></a>Las siguientes características solo están disponibles en ReFS:

| Funcionalidad       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Clonación de bloques | Sí | No |
| VDL disperso | Sí | No |
| Paridad acelerada por reflejo| Sí (en los Espacios de almacenamiento directo) | No |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>Las siguientes características no están disponibles en ReFS en este momento:

| Funcionalidad       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Compresión de sistema de archivos | No | Sí |
| Cifrado de sistema de archivos | No | Sí |
| Transacciones | No | Sí |
| Vínculos físicos | No | Sí |
| Id. de objeto | No | Sí |
| Transferencia de datos descargados (ODX) | No | Sí |
| Nombres cortos | No | Sí |
| Atributos ampliados | No | Sí |
| Cuotas de disco | No | Sí |
| Ejecutable | No | Sí |
| Compatibilidad con archivos de paginación | No | Sí |
| Compatible con medios extraíbles | No | Sí |

## <a name="additional-references"></a>Referencias adicionales

- [Recomendaciones de tamaño de clúster para ReFS y NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Introducción a Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md)
- [Clonación de bloques de ReFS](block-cloning.md)
- [Flujos de integridad de ReFS](integrity-streams.md)
- [Solución de problemas de ReFS con ReFSUtil](../../administration/windows-commands/refsutil.md)
