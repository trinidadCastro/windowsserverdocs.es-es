---
title: Comprender y ver la resincronización de almacenamiento
description: Información detallada sobre cuándo se produce la resincronización de almacenamiento y cómo verla en Windows Server 2019.
keywords: Espacios de almacenamiento directo, resincronización de almacenamiento, resincronización, almacenamiento, S2D
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: d271d92a14278e52a6020c60f96f48b1c8b35871
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465309"
---
# <a name="understand-and-monitor-storage-resync"></a>Comprender y controlar la resincronización de almacenamiento

>Se aplica a: Windows Server 2019

Las alertas de resincronización de almacenamiento son una nueva funcionalidad de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) en Windows Server 2019 que permite a los servicio de mantenimiento producir un error cuando el almacenamiento se está resincronizando. La alerta es útil para notificarle cuando se está produciendo una resincronización, de modo que no se pueden reducir accidentalmente más servidores (lo que puede provocar que se vean afectados varios dominios de error, lo que provoca que el clúster se quede inactivo). 

En este tema se proporciona información general y los pasos para entender y ver la resincronización de almacenamiento en un clúster de conmutación por error de Windows Server con Espacios de almacenamiento directo.

## <a name="understanding-resync"></a>Descripción de resincronización

Comencemos con un ejemplo sencillo para entender cómo el almacenamiento no está sincronizado. Tenga en cuenta que todas las soluciones de almacenamiento distribuido que no comparten nada (solo unidades locales) exhiben este comportamiento. Como verá a continuación, si un nodo de servidor deja de funcionar, sus unidades no se actualizarán hasta que vuelva a estar en línea; esto es cierto para cualquier arquitectura hiperconvergida. 

Supongamos que queremos almacenar la cadena "HELLO". 

![ASCII de cadena "Hello"](media/understand-storage-resync/hello.png)

Asssuming que tenemos una resistencia de reflejo triple, tenemos tres copias de esta cadena. Ahora, si tomamos #1 de servidor temporalmente (para mantenimiento), no podremos acceder a la copia #1.

![No se puede tener acceso a la copia #1](media/understand-storage-resync/copy1.png)

Supongamos que actualizamos nuestra cadena de "Hola" a "ayuda". en este momento.

![ASCII de cadena "ayuda"](media/understand-storage-resync/help.png)

Una vez que actualice la cadena, copie #2 y #3 se actualizarán correctamente. Sin embargo, todavía no se puede tener acceso a la copia #1 porque el servidor #1 está inactivo temporalmente (para mantenimiento). 

![GIF de escritura para copiar #2 y #2 "](media/understand-storage-resync/write.gif)

Ahora tenemos copia #1 que tiene datos que no están sincronizados. El sistema operativo usa el seguimiento granular de regiones desfasadas para realizar un seguimiento de los bits que no están sincronizados. De este modo, cuando el servidor #1 vuelva a estar en línea, podremos sincronizar los cambios leyendo los datos de la copia #2 o #3 y sobrescribiendo los datos en la copia #1. Las ventajas de este enfoque son que solo necesitamos copiar los datos que están obsoletos, en lugar de resincronizar todos los datos de #2 de servidor o de servidor #3.

![GIF de sobrescritura para copiar #1 "](media/understand-storage-resync/overwrite.gif)

Por lo tanto, esto explica cómo se dessincronizan los datos. Pero, ¿qué aspecto tiene en un nivel alto? En este ejemplo, supongamos que tenemos un clúster hiperconvergido de tres servidores. Cuando el servidor #1 esté en mantenimiento, lo verá como inactivo. Al hacer una copia de seguridad del servidor #1, se iniciará la resincronización de todo su almacenamiento mediante el seguimiento granular de la región desfasada (explicado anteriormente). Una vez que todos los datos se sincronizan de nuevo, se mostrarán todos los servidores.

![GIF de la vista de administración de resincronización "](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>Cómo supervisar la resincronización de almacenamiento en Windows Server 2019

Ahora que comprende cómo funciona la resincronización de almacenamiento, echemos un vistazo a cómo se muestra en Windows Server 2019. Hemos agregado un nuevo error al [servicio de mantenimiento](../../failover-clustering/health-service-overview.md) que se mostrará cuando el almacenamiento se esté resincronizando.

Para ver este error en PowerShell, ejecute:

``` PowerShell
Get-HealthFault
```

Se trata de un nuevo error en Windows Server 2019 y aparecerá en PowerShell, en el informe de validación de clúster y en cualquier otra parte que se compile en los errores de estado. 

Para obtener una vista más profunda, puede consultar la base de datos de la serie temporal en PowerShell de la siguiente manera:

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

En particular, el centro de administración de Windows usa errores de estado para establecer el estado y el color de los nodos de clúster. Por lo tanto, este nuevo error hará que los nodos del clúster pasen de rojo (abajo) a amarillo (resincronizando) a verde (arriba), en lugar de ir directamente de rojo a verde, en el panel de HCI.

![Imagen de la vista de resincronización 2016 frente a 2019](media/understand-storage-resync/compare.png)

Al mostrar el progreso general de la resincronización de almacenamiento, puede saber con exactitud la cantidad de datos que no están sincronizados y si el sistema está progresando. Cuando abra el centro de administración de Windows y vaya al *Panel*, verá la nueva alerta de la siguiente manera:

![Imagen de alerta en el centro de administración de Windows "](media/understand-storage-resync/alert.png)

La alerta es útil para notificarle cuando se está produciendo una resincronización, de modo que no se pueden reducir accidentalmente más servidores (lo que puede provocar que se vean afectados varios dominios de error, lo que provoca que el clúster se quede inactivo). 

Si navega a la página *servidores* del centro de administración de Windows, hace clic en *inventario*y, a continuación, elige un servidor específico, puede obtener una vista más detallada de la apariencia de esta resincronización de almacenamiento por servidor. Si navega al servidor y observa el gráfico de *almacenamiento* , verá la cantidad de datos que deben repararse en una línea *púrpura* con el número exacto justo antes. Esta cantidad aumentará cuando el servidor esté inactivo (es necesario volver a sincronizar más datos) y disminuirá gradualmente cuando el servidor vuelva a estar en línea (los datos se están sincronizando). Cuando la cantidad de datos que es necesario reparar es 0, el almacenamiento se realiza de forma gratuita, ya que ahora puede dejar de funcionar un servidor si es necesario. A continuación se muestra una captura de pantalla de esta experiencia en el centro de administración de Windows:

![Imagen de la vista de servidor en el centro de administración de Windows](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>Cómo ver la resincronización de almacenamiento en Windows Server 2016

Como puede ver, esta alerta es especialmente útil para obtener una vista holística de lo que está ocurriendo en la capa de almacenamiento. Resume eficazmente la información que puede obtener del cmdlet Get-StorageJob, que devuelve información sobre los trabajos del módulo de almacenamiento de ejecución prolongada, como una operación de reparación en un espacio de almacenamiento. A continuación se muestra un ejemplo:

```PowerShell
Get-StorageJob
```

Este es el resultado del ejemplo:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Esta vista es mucho más granular, ya que los trabajos de almacenamiento enumerados son por volumen, puede ver la lista de trabajos que se están ejecutando y puede realizar un seguimiento de su progreso individual. Este cmdlet funciona en Windows Server 2016 y 2019.

## <a name="see-also"></a>Vea también

- [Desconectar un servidor para realizar labores de mantenimiento](maintain-servers.md)
- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)