---
title: defrag
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6997e878b2bb7b77a5920ad7398ef7c2301cc8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813196"
---
# <a name="defrag"></a>defrag

>Se aplica a: Windows 10, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Busca y consolida los archivos fragmentados en volúmenes locales para mejorar el rendimiento del sistema.
Pertenecer al grupo local **administradores** , o equivalente, es lo mínimo necesario para ejecutar este comando.

## <a name="syntax"></a>Sintaxis
```
defrag <volumes> | /C | /E <volumes>    [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /A [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /X [/H] [/M [n]| [/U] [/V]]
defrag <volume> [/<Parameter>]*
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|`<volume>`|Especifica la ruta de punto de montaje o letra de unidad del volumen que se va a desfragmentar o analizar.|
|A|Realizar análisis en los volúmenes especificados.|
|C|Realizar la operación en todos los volúmenes.|
|D|Realizar la desfragmentación tradicionales (es decir, el valor predeterminado). Sin embargo, en un volumen en capas tradicional desfragmentación se realiza solo en el nivel de capacidad.|
|E|Realizar la operación en todos los volúmenes, excepto los especificados.|
|G|Optimizar las capas de almacenamiento en los volúmenes especificados.|
|H|Ejecutar la operación con una prioridad normal (valor predeterminado es baja).|
|I n|Optimización de capas se ejecutaría como máximo n segundos en cada volumen.|
|K|Realizar una consolidación de los volúmenes especificados.|
|L|Realizar retrim en los volúmenes especificados.|
|M [n]|Ejecutar la operación en cada volumen en paralelo en segundo plano. Como máximo n subprocesos optimizan las capas de almacenamiento en paralelo.|
|O|Realizar la optimización adecuada para cada tipo de medio.|
|T|Realizar un seguimiento de una operación ya está en curso en el volumen especificado.|
|U|imprimir el progreso de la operación en la pantalla.|
|V|imprimir el resultado detallado que contiene las estadísticas de fragmentación.|
|X|Realizar la consolidación de espacio libre en los volúmenes especificados.|
|?|Muestra esta información de ayuda.|

## <a name="remarks"></a>Comentarios
-   No se puede desfragmentar tipos específicos de unidades o volúmenes del sistema de archivos:
    -   No se puede desfragmentar volúmenes que se ha bloqueado el sistema de archivos.
    -   No se puede desfragmentar volúmenes que el sistema de archivos ha marcado como modificado, lo que indica posibles daños. Debe ejecutar **chkdsk** en un volumen de datos sucio antes de poder desfragmentar. Puede determinar si un volumen está desfasado utilizando el **fsutil** dirty comando de consulta. Para obtener más información acerca de **chkdsk** y **fsutil** con errores, vea [referencias adicionales](defrag.md#BKMK_additionalRef).
    -   No puede desfragmentar unidades de red.
    -   No puede desfragmentar cdrom.
    -   No puede desfragmentar volúmenes de sistema de archivos que no son **NTFS**, **ReFS**, **Fat** o **Fat32**.
-   Con Windows Server 2008 R2, Windows Server 2008 y Windows Vista, puede programar para desfragmentar un volumen. Sin embargo, no se puede programar para desfragmentar un volumen en un disco duro Virtual (VHD) que reside en una SSD o una unidad de estado sólido (SSD).
-   Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio podrían llevar a cabo este procedimiento. Como procedimiento de seguridad recomendado, considere el uso de **ejecución** para llevar a cabo este procedimiento.
-   Un volumen debe tener al menos el 15% espacio libre para **defrag** desfragmentarlo completamente y de manera adecuada. **desfragmentación** usa este espacio como área de ordenación para los fragmentos del archivo. Si un volumen tiene menos de 15% de espacio libre, **defrag** lo desfragmentará solo parcialmente. Para aumentar el espacio libre en un volumen, elimine archivos innecesarios o muévalos a otro disco.
-   Mientras **defrag** es analizar y desfragmentar un volumen, muestra un cursor intermitente. Cuando **defrag** termina de analizar y desfragmentar el volumen, que muestra el informe de análisis, el informe de desfragmentación o ambos informes y, a continuación, se cierra en el símbolo del sistema.
-   De forma predeterminada, **defrag** muestra un resumen de los informes de análisis y de desfragmentación si no especifica la **/a** o **/v** parámetros.
-   Puede enviar los informes en un archivo de texto, escriba **> ***FileName.txt*, donde *archivo.txt* es un nombre de archivo que especifique. Por ejemplo: `defrag volume /v > FileName.txt`
-   Para interrumpir el proceso de desfragmentación, en la línea de comandos, presione **CTRL + C**.
-   Ejecuta el **defrag** comando y Desfragmentador de disco se excluyen mutuamente. Si usas el Desfragmentador de disco para desfragmentar un volumen y ejecutar el **defrag** comando en una línea de comandos, el **defrag** comando produce un error. Por el contrario, si ejecuta el **defrag** comando y abrir el Desfragmentador de disco, las opciones de desfragmentación de Desfragmentador de disco no están disponibles.

## <a name="BKMK_examples"></a>Ejemplos
Para desfragmentar el volumen en la unidad C proporcionando el progreso y la salida detallada, escriba:
```
defrag C: /U /V
```
Para desfragmentar los volúmenes en unidades C y D en paralelo en segundo plano, escriba:
```
defrag C: D: /M
```
Para realizar un análisis de fragmentación de un volumen montado en la unidad C y proporcionar el progreso, escriba:
```
defrag C: mountpoint /A /U
```
Para desfragmentar todos los volúmenes con prioridad normal y proporcionar resultados detallados, escriba:
```
defrag /C /H /V
```

## <a name="BKMK_scheduledTask"></a>Tarea programada
Desfragmentación del programa la tarea se ejecuta como una tarea de mantenimiento y normalmente está programada para ejecutarse cada semana. Administrador puede cambiar la frecuencia de uso **optimizar unidades** aplicación.
- Cuando se ejecuta desde la tarea programada, **defrag** tiene la siguiente directiva para SSDs:
   - **Traditional defrag** (es decir, mover los archivos para que sean contiguas razonablemente) y **retrim** se ejecuta solo una vez al mes.
   - Si ambos **tradicional de desfragmentación** y **retrim** se omiten, **analysis** no se ejecuta.
      - Si el usuario ejecutó **tradicional de desfragmentación** manualmente en una SSD, diga 3 semanas después de la última programada de ejecución de tareas y, después, llevará a cabo la siguiente tarea programada que ejecute **analysis** y **retrim** pero omitir **tradicional de desfragmentación** en esa unidad SSD.
   - Si **analysis** se omite, el **ejecutó por última vez** tiempo en **optimizar unidades** no se actualizará.  Por SSDs el **ejecutó por última vez** tiempo en **optimizar unidades** puede ser un mes de antigüedad.
- Esta tarea de mantenimiento puede desfragmentar todos los volúmenes, en ocasiones porque esta tarea hace lo siguiente:
   - No reactivar el equipo para poder ejecutar la desfragmentación
   - Se inicia sólo si el equipo funciona con corriente Alterna y se detiene si el equipo cambia a alimentación por batería
   - Se detiene si el equipo deja de estar inactivo

## <a name="BKMK_additionalRef"></a>Referencias adicionales
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil dirty](fsutil-dirty.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
