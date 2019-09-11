---
title: Solucionar problemas de rendimiento del administrador de memoria y caché
description: Solucionar problemas de rendimiento del administrador de memoria y caché en Windows Server 16
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 56871924311a945d62fef8a7ef7231889ba018cc
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866406"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Solucionar problemas de rendimiento del administrador de memoria y caché

Antes de Windows Server 2012, dos problemas principales potenciales provocaban que la caché de archivos del sistema crezca hasta que la memoria disponible casi se agotara en ciertas cargas de trabajo. Cuando esta situación da lugar a una lentitud en el sistema, puede determinar si el servidor está enfrente a uno de estos problemas.


## <a name="counters-to-monitor"></a>Contadores para supervisar

-   \\ Duración&lt; media de caché en espera de memoria a largo plazo 1800 segundos

-   Mbytes disponibles de memoria\\insuficientes

-   Bytes residentes de caché del sistema de memoria\\

Si las\\Mbytes disponibles de memoria son bajas y, al mismo\\tiempo, los bytes residentes de caché del sistema de memoria consumen una parte significativa de la memoria física, puede usar [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) para averiguar para qué se usa la memoria caché.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>La caché de archivos del sistema contiene estructuras de datos de metarchivo NTFS


Este problema se indica mediante un número muy alto de páginas de metarchivo activas en la salida de RAMMAP, tal como se muestra en la ilustración siguiente. Es posible que este problema se haya observado en servidores ocupados con millones de archivos a los que se tiene acceso, por lo que los datos del metarchivo NTFS de almacenamiento en caché no se liberan de la memoria caché.

![vista rammap](../../media/perftune-guide-rammap.png)

El problema que ha usado la herramienta *DynCache* para mitigarlo. En Windows Server 2012 +, se ha rediseñado la arquitectura y este problema ya no existe.

## <a name="system-file-cache-contains-memory-mapped-files"></a>La memoria caché de archivos del sistema contiene archivos asignados a memoria


Este problema se indica mediante un número muy alto de páginas de archivos asignados activas en la salida de RAMMAP. Esto normalmente indica que alguna aplicación en el servidor está abriendo muchos archivos grandes mediante la interfaz [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) con la marca\_de\_acceso\_aleatorio del marcador de archivo establecida.

Este problema se describe en detalle en el artículo [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369)de Knowledge base. Marca de\_archivo\_marcador de accesoaleatorioesunasugerenciaparaqueeladministradordecachéMantengalasvistasasignadasdelarchivoenmemorialomáximoposible(hastaqueeladministradordememorianoseñaleunacondicióndememoriainsuficiente).\_ Al mismo tiempo, esta marca indica al administrador de caché que deshabilite la captura previa de datos de archivo.

Esta situación se ha mitigado en cierta medida mediante mejoras en el recorte del conjunto de trabajo en Windows Server 2012 +, pero el propio problema debe ser direccionado principalmente por el proveedor\_de\_la aplicación mediante la no utilización de la marca de archivo Random\_.Acceso. Una solución alternativa para el proveedor de la aplicación podría ser usar una prioridad de memoria baja al acceder a los archivos. Esto puede lograrse mediante la API de [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) . Las páginas a las que se tiene acceso con una prioridad de memoria baja se quitan del espacio de trabajo de forma más agresiva.

El administrador de caché, a partir de Windows Server 2016, reduce aún más este paso omitiendo FILE_FLAG_RANDOM_ACCESS al tomar decisiones de recorte, por lo que se trata como cualquier otro archivo abierto sin la marca FILE_FLAG_RANDOM_ACCESS (el administrador de caché sigue respetando esto marca para deshabilitar la captura previa de datos de archivo. Todavía puede hacer que la memoria caché del sistema se llene si tiene un gran número de archivos abiertos con esta marca y tiene acceso de forma realmente aleatoria. Se recomienda encarecidamente que las aplicaciones no utilicen FILE_FLAG_RANDOM_ACCESS.
