---
title: defrag
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2d930e224ac1610b5e49cbf5701778bfb6f14b2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378712"
---
# <a name="defrag"></a>defrag

>Se aplica a: Windows 10, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Busca y consolida archivos fragmentados en volúmenes locales para mejorar el rendimiento del sistema.
La pertenencia al grupo local **administradores** , o equivalente, es lo mínimo necesario para ejecutar este comando.

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
|`<volume>`|Especifica la letra de unidad o la ruta de acceso del punto de montaje del volumen que se va a desfragmentar o analizar.|
|A|Realizar análisis en los volúmenes especificados.|
|C|Realice la operación en todos los volúmenes.|
|D|Realice la desfragmentación tradicional (este es el valor predeterminado). Sin embargo, en un volumen en capas, la desfragmentación tradicional solo se realiza en el nivel de capacidad.|
|E|Realice la operación en todos los volúmenes excepto en los especificados.|
|G|Optimice las capas de almacenamiento en los volúmenes especificados.|
|H|Ejecute la operación con la prioridad normal (el valor predeterminado es low).|
|N|La optimización de niveles se ejecutaría durante un máximo de n segundos en cada volumen.|
|K|Realice la consolidación de bloques en los volúmenes especificados.|
|L|Realice un recorte en los volúmenes especificados.|
|M [n]|Ejecute la operación en cada volumen en paralelo en segundo plano. Como máximo, n subprocesos optimiza las capas de almacenamiento en paralelo.|
|O|Realice la optimización adecuada para cada tipo de medio.|
|T|Realiza un seguimiento de una operación que ya está en curso en el volumen especificado.|
|5\.50|imprime el progreso de la operación en la pantalla.|
|V|imprime el resultado detallado que contiene las estadísticas de fragmentación.|
|X|Realice la consolidación de espacio libre en los volúmenes especificados.|
|?|Muestra esta información de ayuda.|

## <a name="remarks"></a>Comentarios
- No se pueden desfragmentar tipos específicos de volúmenes o unidades del sistema de archivos:
  -   No se pueden desfragmentar los volúmenes bloqueados por el sistema de archivos.
  -   No puede desfragmentar los volúmenes que el sistema de archivos marcó como modificados, lo que indica posibles daños. Debe ejecutar **CHKDSK** en un volumen sucio antes de poder desfragmentarlo. Puede determinar si un volumen se ha modificado mediante el comando **fsutil** Dirty Query. Para obtener más información acerca de **CHKDSK** y **fsutil** Dirty, consulte [referencias adicionales](defrag.md#BKMK_additionalRef).
  -   No se pueden desfragmentar las unidades de red.
  -   No puede desfragmentar cdROMs.
  -   No se pueden desfragmentar volúmenes del sistema de archivos que no sean **NTFS**, **ReFS**, **FAT** o **FAT32**.
- Con Windows Server 2008 R2, Windows Server 2008 y, Windows Vista, puede programar la desfragmentación de un volumen. Sin embargo, no se puede programar para desfragmentar una unidad de estado sólido (SSD) o un volumen en un disco duro virtual (VHD) que resida en una SSD.
- Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores en el equipo local o tener delegada la autoridad correspondiente. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio podrían llevar a cabo este procedimiento. Como práctica recomendada de seguridad, considere la posibilidad de usar **Ejecutar como** para realizar este procedimiento.
- Un volumen debe tener al menos un 15% de espacio libre para que **Defrag** se desfragmente de forma completa y adecuada. **Defrag** usa este espacio como área de ordenación para los fragmentos de archivo. Si un volumen tiene menos del 15% de espacio libre, **Defrag** solo lo desfragmentará parcialmente. Para aumentar el espacio libre en un volumen, elimine los archivos innecesarios o muévalos a otro disco.
- Mientras que **Defrag** está analizando y desfragmentando un volumen, muestra un cursor parpadeante. Cuando **Defrag** finaliza el análisis y desfragmentación del volumen, muestra el informe de análisis, el informe de desfragmentación o ambos informes y, a continuación, sale del símbolo del sistema.
- De forma predeterminada, **Defrag** muestra un resumen de los informes de análisis y desfragmentación si no se especifican los parámetros **/a** o **/v** .
- Puede enviar los informes a un archivo de texto escribiendo **>** <em>filename. txt</em>, donde *filename. txt* es el nombre de archivo que especifique. Por ejemplo: `defrag volume /v > FileName.txt`
- Para interrumpir el proceso de desfragmentación, en la línea de comandos, presione **Ctrl + C**.
- La ejecución del comando **Defrag** y del Desfragmentador de disco es mutuamente excluyente. Si usa el Desfragmentador de disco para desfragmentar un volumen y ejecuta el comando **Defrag** en una línea de comandos, se produce un error en el comando **Defrag** . Por el contrario, si ejecuta el comando **Defrag** y abre el Desfragmentador de disco, las opciones de desfragmentación del Desfragmentador de disco no estarán disponibles.

## <a name="BKMK_examples"></a>Example
Para desfragmentar el volumen de la unidad C a la vez que proporciona el progreso y el resultado detallado, escriba:
```
defrag C: /U /V
```
Para desfragmentar los volúmenes en las unidades C y D en paralelo en segundo plano, escriba:
```
defrag C: D: /M
```
Para realizar un análisis de fragmentación de un volumen montado en la unidad C y proporcionar el progreso, escriba:
```
defrag C: mountpoint /A /U
```
Para desfragmentar todos los volúmenes con prioridad normal y proporcionar una salida detallada, escriba:
```
defrag /C /H /V
```

## <a name="BKMK_scheduledTask"></a>Tarea programada
La tarea programada de Defrag se ejecuta como una tarea de mantenimiento y normalmente está programada para ejecutarse cada semana. El administrador puede cambiar la frecuencia con la aplicación **optimizar unidades** .
- Cuando se ejecuta desde la tarea programada, **Defrag** tiene la Directiva siguiente para las SSD:
   - La **desfragmentación tradicional** (es decir, mover archivos para que sean razonablemente contiguos) y **recortar** se ejecuta solo una vez al mes.
   - Si se omiten la **desfragmentación tradicional** y el **recorte** , no se ejecuta el **análisis** .
      - Si el usuario ejecutó la **desfragmentación tradicional** manualmente en una SSD, por ejemplo 3 semanas después de la última ejecución de la tarea programada, la siguiente ejecución de la tarea programada llevará a cabo el **análisis** y el **recorte** , pero omitirá la **desfragmentación tradicional** en esa SSD.
   - Si se omite el **análisis** , no se actualizará la hora de la **última ejecución** en **optimizar unidades** .  Por lo tanto, para las SSD, la hora de la **última ejecución** en **Optimize Drives** puede ser un mes anterior.
- Esta tarea de mantenimiento podría no desfragmentar todos los volúmenes a veces porque esta tarea hace lo siguiente:
   - No reactiva el equipo para ejecutar Defrag
   - Solo se inicia si el equipo está conectado a la corriente alterna y se detiene si el equipo cambia a la energía de la batería
   - Detiene si el equipo deja de estar inactivo

## <a name="BKMK_additionalRef"></a>Referencias adicionales
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil Dirty](fsutil-dirty.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
