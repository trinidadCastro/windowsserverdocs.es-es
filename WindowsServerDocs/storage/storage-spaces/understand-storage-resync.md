---
title: Comprender y ver resincronización de almacenamiento
description: Obtener información detallada sobre cuando se produce la resincronización de almacenamiento y cómo verla en Windows Server 2019.
keywords: Espacios de almacenamiento directo, resincronización de almacenamiento, resincronizar, almacenamiento, S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 748eccd10fc0c3c962d6e64ff6ead08017ac1947
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "9009666"
---
# Comprender y controlar la resincronización de almacenamiento

>Se aplica a: Windows Server 2019

Las alertas de resincronización de almacenamiento son una nueva funcionalidad de [Espacios de almacenamiento directo](storage-spaces-direct-overview.md) en Windows Server 2019 que permite que el servicio de mantenimiento producir un error cuando el sistema de almacenamiento está en proceso de resincronización. La alerta es útil para notificar está ocurriendo resincronización, para que no se toma accidentalmente más servidores hacia abajo (que podría provocar que varios dominios de error se vea afectado, lo que provoca que el clúster ir hacia abajo). 

En este tema se proporciona en segundo plano y los pasos para comprender y ver resincronización de almacenamiento en un clúster de conmutación por error de Windows Server con espacios de almacenamiento directo.

## Resincronización de descripción

Empecemos con un ejemplo sencillo para comprender cómo el almacenamiento obtiene sincronizado. Ten en cuenta que cualquier compartir nada (unidades locales solo) solución de almacenamiento distribuido tiene este comportamiento. Como se verá más adelante, si un nodo de servidor deja de funcionar, a continuación, no se actualizarán sus unidades hasta que vuelva a estar conectado - esto sucede en cualquier arquitectura hiperconvergida. 

Supongamos que queremos almacenar la cadena "HELLO". 

![ASCII de cadena "Hola"](media/understand-storage-resync/hello.png)

Asssuming que tenemos la resistencia de reflejo triple, tenemos tres copias de esta cadena. Ahora, si tomamos servidor #1 hacia abajo temporalmente (para el mantenimiento), a continuación, no podemos accedemos copia #1.

![No se puede obtener acceso a la copia #1](media/understand-storage-resync/copy1.png)

Supongamos que actualizamos nuestra cadena de "Hola" a "Ayuda!" en este momento.

![ASCII de cadena "Ayuda!"](media/understand-storage-resync/help.png)

Una vez que actualizamos la cadena, copia #2 y 3 # será actualizado correctamente. Sin embargo, copia #1 aún no es accesible porque el servidor 1 # está inactivo temporalmente (para el mantenimiento). 

![GIF de escritura al copiar #2 y #2 "](media/understand-storage-resync/write.gif)

Ahora, tenemos que copiar #1, que tiene los datos que están sincronizados. El sistema operativo usa la región obsoleta detallado de seguimiento para realizar un seguimiento de las partes que no están sincronizadas. De este modo, cuando el servidor 1 # vuelva a estar conectado, podemos sincronice los cambios mediante la lectura de los datos de copia #2 o 3 # y sobrescribir los datos de copia #1. Las ventajas de este enfoque son que necesitamos solo hay que copiar los datos que están obsoletos, en lugar de todos los datos desde el servidor #2 o 3 # en un proceso de resincronización..

![GIF de sobrescribir copiar #1 "](media/understand-storage-resync/overwrite.gif)

Por lo tanto, esto explica cómo obtiene deja de sincronizar los datos. Pero, ¿qué aspecto tiene en un nivel alto? Supone para este ejemplo que tenemos un clúster hiperconvergido de tres servidores. Cuando el servidor 1 # está en mantenimiento, verás como hacia abajo. Cuando el servidor 1 # copia de seguridad, se iniciará todo su almacenamiento mediante el seguimiento detallado región obsoleta (explicado anteriormente) en un proceso de resincronización.. Una vez todas sincronizados los datos, se mostrarán todos los servidores como máximo.

![GIF de vista de administración de resincronización"](media/understand-storage-resync/admin.gif)

## Cómo supervisar resincronización de almacenamiento en Windows Server 2019

Ahora que conoces cómo funciona la resincronización de almacenamiento, echemos un vistazo a cómo esto se muestra en Windows Server 2019. Hemos agregado una tolerancia nuevo al [Servicio de mantenimiento](../../failover-clustering/health-service-overview.md) que se mostrará cuando el sistema de almacenamiento está en proceso de resincronización.

Para ver este error en PowerShell, ejecute:

``` PowerShell
Get-HealthFault
```

Esto es un error de nuevo en Windows Server 2019 y aparecerá en PowerShell, en el informe de validación de clúster y cualquier otro que se basa en los errores de estado. 

Para obtener una vista más profunda, puedes consultar la base de datos de serie de tiempo en PowerShell como sigue:

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

En concreto, Windows Admin Center usa errores de estado para establecer el estado y el color de los nodos del clúster. Por lo tanto, este error de nuevo hará que los nodos del clúster realizar la transición de rojo (abajo) a amarillo (proceso de resincronización) a verde (arriba), en lugar de ir directamente de rojo a verde, en el panel de HCI.

![Imagen de vista de 2016 vs 2019 de resincronización"](media/understand-storage-resync/compare.png)

Mostrando el progreso de resincronización de almacenamiento de información general, puede saber con exactitud la cantidad de datos es sincronizado y si el sistema avanza. Cuando se abre Windows Admin Center y ve al *panel*, verás la nueva alerta como sigue:

![Imagen de alerta en Windows Admin Center"](media/understand-storage-resync/alert.png)

La alerta es útil para notificar está ocurriendo resincronización, para que no se toma accidentalmente más servidores hacia abajo (que podría provocar que varios dominios de error se vea afectado, lo que provoca que el clúster ir hacia abajo). 

Si navegar a la página de *servidores* en Windows Admin Center, haz clic en el *inventario*y, a continuación, elige un servidor determinado, puedes obtener una vista más detallada del aspecto de este resincronización de almacenamiento por servidor. Si se navega hasta el servidor y mira el gráfico de *almacenamiento* , aparecerá la cantidad de datos que necesita repararse en una línea con el número exacto *púrpura* justo encima. Esta cantidad aumentará cuando el servidor está hacia abajo (más datos deben sincronizarse) y disminuir gradualmente cuando el servidor vuelva a estar conectado (es que se sincronizan datos). Cuando la cantidad de datos que deben ser reparación es 0, el sistema de almacenamiento está realiza el proceso de resincronización - ya estás libre provocará un servidor si es necesario. Captura de pantalla de esta experiencia en Windows Admin Center se muestra a continuación:

![Imagen de vista de servidor en Windows Admin Center"](media/understand-storage-resync/server.png)

## Cómo ver resincronización de almacenamiento en Windows Server 2016

Como puedes ver, esta alerta es especialmente útil para obtener una visión global de lo que sucede en la capa de almacenamiento. Resume eficazmente la información que se puede acceder desde el cmdlet Get-StorageJob, que devuelve información acerca de los trabajos de módulo de almacenamiento de larga ejecución, como una operación de reparación en un espacio de almacenamiento. A continuación se muestra un ejemplo:

```PowerShell
Get-StorageJob
```

Este es el resultado de ejemplo:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Esta vista es mucho más detallada debido a que los trabajos de almacenamiento enumerados por volumen, puedes ver la lista de tareas que se ejecutan y puedes hacer un seguimiento de su progreso individual. Este cmdlet funciona en Windows Server 2016 y 2019.

## Ver también

- [Desconectar un servidor para realizar labores de mantenimiento](maintain-servers.md)
- [Información general de Espacios de almacenamiento directos](storage-spaces-direct-overview.md)