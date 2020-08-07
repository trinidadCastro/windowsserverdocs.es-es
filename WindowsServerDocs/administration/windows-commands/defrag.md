---
title: defrag
description: Artículo de referencia para el comando Defrag, que busca y consolida archivos fragmentados en volúmenes locales para mejorar el rendimiento del sistema.
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c68edbb4511df12912adbc666201d5a381c06fe3
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891482"
---
# <a name="defrag"></a>defrag

> Se aplica a: Windows 10, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Busca y consolida archivos fragmentados en volúmenes locales para mejorar el rendimiento del sistema.

La pertenencia al grupo local **administradores** , o equivalente, es lo mínimo necesario para ejecutar este comando.

## <a name="syntax"></a>Sintaxis

```
defrag <volumes> | /c | /e <volumes>    [/h] [/m [n]| [/u] [v]]
defrag <volumes> | /c | /e <volumes> /a [/h] [/m [n]| [/u] [v]]
defrag <volumes> | /c | /e <volumes> /x [/h] [/m [n]| [/u] [v]]
defrag <volume> [<parameters>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<volume>` | Especifica la letra de unidad o la ruta de acceso del punto de montaje del volumen que se va a desfragmentar o analizar. |
| /a | Realizar análisis en los volúmenes especificados. |
| /C | Realice la operación en todos los volúmenes. |
| /d | Realice la desfragmentación tradicional (este es el valor predeterminado). Sin embargo, en un volumen en capas, la desfragmentación tradicional solo se realiza en el nivel de capacidad. |
| /e | Realice la operación en todos los volúmenes excepto en los especificados. |
| /g | Optimice las capas de almacenamiento en los volúmenes especificados. |
| /h | Ejecute la operación con la prioridad normal (el valor predeterminado es low). |
| /i [n] | La optimización de niveles se ejecutaría durante un máximo de n segundos en cada volumen. |
| /k | Realice la consolidación de bloques en los volúmenes especificados. |
| /l | Realice un recorte en los volúmenes especificados. |
| /m [n] | Ejecute la operación en cada volumen en paralelo en segundo plano. Como máximo, n subprocesos optimiza las capas de almacenamiento en paralelo. |
| /o | Realice la optimización adecuada para cada tipo de medio. |
| /t | Realiza un seguimiento de una operación que ya está en curso en el volumen especificado. |
| /U | Imprime el progreso de la operación en la pantalla. |
| /v | Imprime el resultado detallado que contiene las estadísticas de fragmentación. |
| /x | Realice la consolidación de espacio libre en los volúmenes especificados. |
| /? | Muestra esta información de ayuda. |

#### <a name="remarks"></a>Observaciones

- No se pueden desfragmentar unidades o volúmenes del sistema de archivos específicos, como:

  - Volúmenes bloqueados por el sistema de archivos.

  - Volúmenes del sistema de archivos marcados como modificados, lo que indica posibles daños.<br>Debe ejecutar `chkdsk` antes de poder desfragmentar este volumen o unidad. Puede determinar si un volumen se ha modificado mediante el `fsutil dirty` comando.

  - Unidades de red.

  - CD-ROM.

  - Volúmenes del sistema de archivos que no son **NTFS**, **ReFS**, **FAT** o **FAT32**.

- No se puede programar para desfragmentar una unidad de estado sólido (SSD) o un volumen en un disco duro virtual (VHD) que resida en una SSD.

- Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores del equipo local o tener delegada la autoridad adecuada. Si el equipo está unido a un dominio, los miembros del grupo Administradores de dominio podrían llevar a cabo este procedimiento. Como práctica recomendada de seguridad, considere la posibilidad de usar **Ejecutar como** para realizar este procedimiento.

- Un volumen debe tener al menos un 15% de espacio libre para que **Defrag** se desfragmente de forma completa y adecuada. **Defrag** usa este espacio como área de ordenación para los fragmentos de archivo. Si un volumen tiene menos del 15% de espacio libre, **Defrag** solo lo desfragmentará parcialmente. Para aumentar el espacio libre en un volumen, elimine los archivos innecesarios o muévalos a otro disco.

- Mientras que **Defrag** está analizando y desfragmentando un volumen, muestra un cursor parpadeante. Cuando **Defrag** finaliza el análisis y desfragmentación del volumen, muestra el informe de análisis, el informe de desfragmentación o ambos informes y, a continuación, sale del símbolo del sistema.

- De forma predeterminada, **Defrag** muestra un resumen de los informes de análisis y desfragmentación si no se especifican los parámetros **/a** o **/v** .

- Puede enviar los informes a un archivo de texto escribiendo **>** <em>FileName.txt</em>, donde *FileName.txt* es el nombre de archivo que especifique. Por ejemplo: `defrag volume /v > FileName.txt`

- Para interrumpir el proceso de desfragmentación, en la línea de comandos, presione **Ctrl + C**.

- La ejecución del comando **Defrag** y del Desfragmentador de disco es mutuamente excluyente. Si usa el Desfragmentador de disco para desfragmentar un volumen y ejecuta el comando **Defrag** en una línea de comandos, se produce un error en el comando **Defrag** . Por el contrario, si ejecuta el comando **Defrag** y abre el Desfragmentador de disco, las opciones de desfragmentación del Desfragmentador de disco no estarán disponibles.

## <a name="examples"></a>Ejemplos

Para desfragmentar el volumen de la unidad C a la vez que proporciona el progreso y el resultado detallado, escriba:

```
defrag c: /u /v
```

Para desfragmentar los volúmenes en las unidades C y D en paralelo en segundo plano, escriba:

```
defrag c: d: /m
```

Para realizar un análisis de fragmentación de un volumen montado en la unidad C y proporcionar el progreso, escriba:

```
defrag c: mountpoint /a /u
```

Para desfragmentar todos los volúmenes con prioridad normal y proporcionar una salida detallada, escriba:

```
defrag /c /h /v
```

## <a name="scheduled-task"></a>Tarea programada

El proceso de desfragmentación ejecuta la tarea programada como una tarea de mantenimiento, que normalmente se ejecuta cada semana. Como administrador, puede cambiar la frecuencia con la que se ejecuta la tarea mediante la aplicación **optimizar unidades** .

- Cuando se ejecuta desde la tarea programada, **Defrag** usa las siguientes directrices de directiva para SSD:

  - **Procesos de optimización tradicionales**. Incluye **desfragmentación tradicional**, por ejemplo, mover archivos para que sean razonablemente contiguos y **recortar**. Esto se hace una vez al mes. Sin embargo, si se omiten la **desfragmentación tradicional** y el **recorte** , el **análisis** no se ejecuta.

  - Si ejecuta manualmente la **desfragmentación tradicional** en una SSD, entre las ejecuciones programadas normalmente, la siguiente ejecución de la tarea programada realiza el **análisis** y el **recorte**, pero omite la **desfragmentación tradicional** en esa SSD.

  - Si omite el **análisis**, no verá un último tiempo de **ejecución** actualizado en la aplicación **optimizar unidades** . Por eso, el último tiempo de **ejecución** puede ser hasta un mes anterior.

  - Es posible que la tarea programada no desfragmente todos los volúmenes. Esto suele deberse a que:

    - El proceso no reactivará el equipo para ejecutarse.

    - El equipo no está conectado. El proceso no se ejecutará si el equipo está funcionando con batería.

    - El equipo inició la copia de seguridad (se reanudó de inactivo).

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [chkdsk](chkdsk.md)

- [fsutil](fsutil.md)

- [fsutil dirty](fsutil-dirty.md)

- [Optimizar el volumen de PowerShell](/powershell/module/storage/optimize-volume?view=win10-ps)
