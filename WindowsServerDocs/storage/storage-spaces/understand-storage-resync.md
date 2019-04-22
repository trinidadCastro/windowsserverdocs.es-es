---
title: Entender y comprobar la resincronización de almacenamiento
description: Información detallada sobre cuando ocurra la resincronización de almacenamiento y cómo verlo en Windows Server 2019.
keywords: Espacios de almacenamiento directo, la resincronización de almacenamiento, vuelva a sincronizar, almacenamiento, S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813466"
---
# <a name="understand-and-monitor-storage-resync"></a>Comprender y controlar la resincronización de almacenamiento

>Se aplica a: Windows Server 2019

Las alertas de resincronización de almacenamiento son una nueva funcionalidad de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) en Windows Server 2019 que permite que el servicio de mantenimiento producir un error cuando se está resincronizando su almacenamiento. La alerta es útil para que le avise cuando se produce la resincronización, por lo que no tomar accidentalmente más servidores abajo (lo que podría provocar que varios dominios de error se ven afectadas, lo que resulta en el clúster deja de funcionar). 

En este tema se proporciona en segundo plano y los pasos para entender y comprobar la resincronización de almacenamiento en un clúster de conmutación por error de Windows Server con espacios de almacenamiento directo.

## <a name="understanding-resync"></a>Resincronización de descripción

Comencemos con un ejemplo sencillo para comprender cómo se queda sin sincronizarse almacenamiento. Tenga en cuenta que cualquier nada compartido (solo las unidades locales) solución de almacenamiento distribuido tiene este comportamiento. Como verá a continuación, si un nodo servidor deja de funcionar, a continuación, sus unidades no se actualizarán hasta que vuelva a conectarla - esto es cierto para cualquier arquitectura hiperconvergido. 

Supongamos que queremos almacenar la cadena "HELLO". 

![ASCII de la cadena "hello"](media/understand-storage-resync/hello.png)

Asssuming contamos con resistencia reflejada en tres vías, tenemos tres copias de esta cadena. Ahora, si tomamos servidor #1 temporalmente (por mantenimiento), a continuación, nos no podemos acceder a copia #1.

![No se puede tener acceso a la copia #1](media/understand-storage-resync/copy1.png)

¡Suponga que se actualice la cadena de "Hola" a "HELP"! en este momento.

![ASCII de la cadena "help".](media/understand-storage-resync/help.png)

Una vez que se actualiza la cadena, copia #2 y 3 # será actualizado correctamente. Sin embargo, copia #1 aún no se puede tener acceso porque servidor #1 está inactivo temporalmente (por mantenimiento). 

![GIF de escritura en copiar #2 y #2 "](media/understand-storage-resync/write.gif)

Ahora, tenemos copia #1, lo que tiene datos que no están sincronizados. El sistema operativo usa granular el seguimiento de regiones para realizar un seguimiento de los bits que no están sincronizados. Este modo al servidor #1 vuelve a conectarse, los cambios que podamos sincronizar por leer los datos de copia #2 o 3 # y sobrescribir los datos de copia #1. Las ventajas de este enfoque son que únicamente se necesita que se copiarán los datos que están obsoletas, en lugar de todos los datos desde el servidor #2 o 3 # resincronizar.

![GIF de sobrescritura para copiar #1 "](media/understand-storage-resync/overwrite.gif)

Por lo tanto, esto explica cómo los datos se ponen sin sincronizar. Pero, ¿qué quiere decir esto en un nivel alto? Suponga para este ejemplo, tenemos un clúster hiperconvergido de tres servidores. Cuando el servidor #1 está en mantenimiento, lo verá como si estuviera inactivo. Cuando vuelve a poner el servidor #1 copia de seguridad, se iniciará resincronizar todo su almacenamiento usa el seguimiento de región sucia granular (que se explicaron anteriormente). Una vez que los datos son todos volvieron sincronizados, se mostrarán todos los servidores como máximo.

![GIF de vista de administración de resincronización"](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>Cómo supervisar la resincronización de almacenamiento en Windows Server 2019

Ahora que comprende cómo funciona la resincronización de almacenamiento, echemos un vistazo a cómo esto se muestra en Windows Server 2019. Hemos agregado un nuevo error en el [servicio de mantenimiento](../../failover-clustering/health-service-overview.md) que se mostrará cuando se está resincronizando su almacenamiento.

Para ver este error en PowerShell, ejecute:

``` PowerShell
Get-HealthFault
```

Esto es un nuevo error en Windows Server 2019 y aparecerá en PowerShell, en el informe de validación de clúster y cualquier otro que se basa en los errores de estado. 

Para obtener una vista más detallada, puede consultar la base de datos de series temporales de PowerShell como sigue:

```PowerShell
Get-ClusterNode | Get-ClusterPerf -ClusterNodeSeriesName ClusterNode.Storage.Degraded
```
Este es un ejemplo de los resultados:

```
Object Description: ClusterNode Server1

Series                       Time                Value Unit
------                       ----                ----- ----
ClusterNode.Storage.Degraded 01/11/2019 16:26:48     214 GB
```

En concreto, Windows Admin Center usa errores de estado para establecer el estado y el color de los nodos del clúster. Por lo tanto, este nuevo error hará que los nodos del clúster para realizar la transición de rojo (abajo) a amarillo (resincronizar) a verde (arriba), en lugar de pasar directamente de rojo a verde, en el panel de HCL.

![Imagen de vista de vs 2016 2019 de resincronización"](media/understand-storage-resync/compare.png)

Al mostrar el progreso de resincronización de almacenamiento general, puede saber con exactitud la cantidad de datos está sincronizada y si el sistema está realizando un progreso hacia delante. Al abrir Windows Admin Center y vaya a la *panel*, verá la nueva alerta como sigue:

![Imagen de alerta en Windows Admin Center "](media/understand-storage-resync/alert.png)

La alerta es útil para que le avise cuando se produce la resincronización, por lo que no tomar accidentalmente más servidores abajo (lo que podría provocar que varios dominios de error se ven afectadas, lo que resulta en el clúster deja de funcionar). 

Si navega a la *servidores* página Windows Admin Center, haga clic en *inventario*y, a continuación, elija un servidor específico, puede obtener una vista más detallada de cómo se ve esta resincronización de almacenamiento en cada servidor. Si navega a su servidor y examine el *almacenamiento* gráfico, verá la cantidad de datos que tiene que repararse en un *púrpura* línea con el número exacto justo arriba. Esta cantidad aumentará cuando el servidor está inactivo (más datos debe ser resynced) y reducir gradualmente cuando vuelve a conectar el servidor (se sincronizan datos). Cuando la cantidad de datos que deba estar reparación es 0, el almacenamiento está hecho resincronizar - ahora es libre de desconectar un servidor hacia abajo si necesita. A continuación se muestra una captura de pantalla de esta experiencia en Windows Admin Center:

![Imagen de vista de servidor en Windows Admin Center "](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>Visualización de resincronización de almacenamiento en Windows Server 2016

Como puede ver, esta alerta es especialmente útil para obtener una vista holística de lo que sucede en la capa de almacenamiento. Eficazmente resume la información que puede obtener desde el cmdlet Get-StorageJob, que devuelve información sobre los trabajos de módulo del almacenamiento de larga ejecución, como una operación de reparación en un espacio de almacenamiento. A continuación se muestra un ejemplo:

```PowerShell
Get-StorageJob
```

Este es el resultado de ejemplo:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Esta vista es mucho más granular, puesto que los trabajos de almacenamiento enumerados son por volumen, puede ver la lista de trabajos que se está ejecutando y puede realizar un seguimiento de su progreso individual. Este cmdlet funciona en Windows Server 2016 y de 2019.

## <a name="see-also"></a>Vea también

- [Poner un servidor sin conexión para el mantenimiento](maintain-servers.md)
- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)