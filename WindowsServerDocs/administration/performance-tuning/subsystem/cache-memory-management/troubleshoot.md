---
title: Solucionar problemas de rendimiento de administrador de memoria y caché
description: Solucionar problemas de memoria caché y problemas de rendimiento del Administrador de memoria en Windows Server 16
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 66c7e2a6b264a837c65df927b271fadd2672fa24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835796"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Solucionar problemas de rendimiento de administrador de memoria y caché

Antes de Windows Server 2012, dos posibles problemas de principales provocó la memoria caché de archivos de sistema crezca hasta casi se ha agotado la memoria disponible en ciertas cargas de trabajo. Cuando los resultados de esta situación en el sistema está lento, puede determinar si el servidor es accesible desde uno de estos problemas.


## <a name="counters-to-monitor"></a>Contadores para supervisar

-   Memoria\\duración en caché de modo de espera a largo plazo promedio (s) &lt; 1800 segundos

-   Memoria\\Mbytes disponibles es baja

-   Memoria\\Bytes residentes de caché del sistema

Si memoria\\Mbytes disponibles es bajo y al mismo tiempo memoria\\Bytes residentes de caché del sistema consume una parte significativa de la memoria física, puede usar [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) para averiguar lo que la memoria caché se está usando para.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>Caché del sistema de archivos contiene estructuras de datos de metarchivo NTFS


Este problema se indica mediante un número muy elevado de las páginas de metarchivo active en RAMMAP de salida, como se muestra en la ilustración siguiente. Este problema es posible que se han observado en servidores ocupados con millones de archivos que se obtiene acceso, lo que resultante en el almacenamiento en caché datos de metarchivo NTFS no se liberan de la memoria caché.

![vista rammap](../../media/perftune-guide-rammap.png)

El problema que se usa para mitigar por *DynCache* herramienta. En Windows Server 2012 y versiones posteriores, se ha rediseñado la arquitectura y este problema ya no debe existir.

## <a name="system-file-cache-contains-memory-mapped-files"></a>Caché del sistema de archivos contiene archivos asignados en memoria


Este problema se indica mediante un número muy elevado de páginas active de archivo asignado en salida RAMMAP. Esto indica normalmente que algunas aplicaciones en el servidor está abriendo un lote de archivos grandes con [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) API con el archivo\_marca\_RANDOM\_acceso marca establecida.

Este problema se describe en detalle en el artículo KB [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369). ARCHIVO\_marca\_RANDOM\_marca de acceso es una sugerencia para el Administrador de caché para mantener las vistas asignadas del archivo en memoria siempre que sea posible (hasta que el Administrador de memoria no indicar la condición de memoria insuficiente). Al mismo tiempo, esta marca indica al administrador de caché para deshabilitar la captura previa de datos de archivo.

Esta situación se ha mitigado hasta cierto punto mediante la colaboración conjunto recorte mejoras en Windows Server 2012 y versiones posteriores, pero el problema propio debe abordar principalmente por el proveedor de la aplicación sin usar archivo\_marca\_RANDOM\_Acceso. Una solución alternativa para el proveedor de la aplicación podría ser utilizar la prioridad de memoria insuficiente al obtener acceso a los archivos. Esto puede lograrse mediante el [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) API. Páginas que se accede con prioridad de memoria baja se quitan desde el conjunto de forma más agresiva de trabajo.

Administrador de caché, a partir de Windows Server 2016 más mitiga omitiendo FILE_FLAG_RANDOM_ACCESS al tomar decisiones de recorte, por lo que se trata igual que cualquier otro archivo que se puede abrir sin la marca FILE_FLAG_RANDOM_ACCESS (Administrador de caché aún respeta esta marca para deshabilitar la captura previa de datos de archivo). Todavía puede provocar inundación de memoria caché del sistema si tiene gran cantidad de archivos abierto con esta marca y acceder de manera verdaderamente aleatoria. Se recomienda que no utiliza FILE_FLAG_RANDOM_ACCESS por las aplicaciones.
