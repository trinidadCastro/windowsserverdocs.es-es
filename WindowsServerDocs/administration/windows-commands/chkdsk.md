---
title: chkdsk
description: Comando comandos de Windows para chkdsk, que comprueba los metadatos del sistema de archivos y del sistema de archivos de un volumen en busca de errores lógicos y físicos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 62912a3c-d2cc-4ef6-9679-43709a286035
author: jasongerend
ms.author: jgerend
manager: lizapo
ms.date: 10/09/2019
ms.openlocfilehash: 130b51e472ebf3d900186d6d63e318c88a340579
ms.sourcegitcommit: e2964a803cba1b8037e10d065a076819d61e8dbe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72252353"
---
# <a name="chkdsk"></a>chkdsk

Comprueba los metadatos del sistema de archivos y del sistema de archivos de un volumen en busca de errores lógicos y físicos. Si se usa sin parámetros, **CHKDSK** solo muestra el estado del volumen y no corrige ningún error. Si se usa con los parámetros **/f**, **/r**, **/x**o **/b** , corrige los errores en el volumen.

> [!IMPORTANT]
> La pertenencia al grupo local **administradores** , o equivalente, es lo mínimo necesario para ejecutar **CHKDSK**. Para abrir una ventana del símbolo del sistema como administrador, haga clic con el botón secundario en **símbolo del sistema** en el menú Inicio y, a continuación, haga clic en **Ejecutar como administrador**.

> [!IMPORTANT]
> No se recomienda interrumpir **CHKDSK** . Sin embargo, cancelar o interrumpir **CHKDSK** no debe dejar el volumen más dañado de lo que era antes de ejecutar **CHKDSK** . La reejecución de **CHKDSK** comprueba y repara cualquier daño restante en el volumen.

> [!IMPORTANT]
> **Nota:** CHKDSK solo se puede usar para discos locales. El comando no se puede usar con una letra de unidad local que se ha redirigido a través de la red.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
chkdsk [<Volume>[[<Path>]<FileName>]] [/f] [/v] [/r] [/x] [/i] [/c] [/l[:<Size>]] [/b]  
```

## <a name="parameters"></a>Parámetros

|      Parámetro       |                  Descripción                                    |
| -------------------- | ------------------------------------------------------------------------ |
|      @no__t 0Volume >      | Especifica la letra de unidad (seguida de un signo de dos puntos), un punto de montaje o un nombre de volumen.  |
| [\<Path >] <FileName> | Use solo con la tabla de asignación de archivos (FAT) y FAT32. Especifica la ubicación y el nombre de un archivo o un conjunto de archivos en los que desea que **CHKDSK** Compruebe la fragmentación. Puede **usar el** y **&#42;** caracteres comodín para especificar varios archivos. |
|         /f          | Corrige errores en el disco. El disco debe estar bloqueado. Si **CHKDSK** no puede bloquear la unidad, aparece un mensaje que le pregunta si desea comprobar la unidad la próxima vez que reinicie el equipo. |
|         /v          | Muestra el nombre de cada archivo en cada directorio a medida que se comprueba el disco.     |
|         /r          | Busca sectores dañados y recupera información legible. El disco debe estar bloqueado. **/r** incluye la funcionalidad de **/f**, con el análisis adicional de los errores de disco físico.                                   |
|         /x          | Fuerza el desmontaje del volumen en primer lugar, si es necesario. Se invalidan todos los identificadores abiertos en la unidad. **/x** también incluye la funcionalidad de **/f**.  |
|         /i          | Úselo solo con NTFS. Realiza una comprobación menos exhaustiva de entradas de índice, lo que reduce la cantidad de tiempo necesario para ejecutar **CHKDSK**.  |
|         /c          | Úselo solo con NTFS. No comprueba los ciclos dentro de la estructura de carpetas, lo que reduce la cantidad de tiempo necesario para ejecutar **CHKDSK**.  |
|    /l [: \<Size >]     | Úselo solo con NTFS. Cambia el tamaño del archivo de registro al tamaño que escriba. Si omite el parámetro size, **/l** muestra el tamaño actual. |
|         b          | Solo NTFS: Borra la lista de clústeres defectuosos en el volumen y vuelve a examinar todos los clústeres asignados y libres en busca de errores. **/b** incluye la funcionalidad de **/r**. Utilice este parámetro después de la creación de imágenes de un volumen en una nueva unidad de disco duro.            |
| /Scan               | Solo NTFS: Ejecuta un examen en línea en el volumen. |
| /forceofflinefix    | Solo NTFS: (Debe usarse con "/SCAN"). Omitir todas las reparaciones en línea; todos los defectos encontrados se ponen en cola para la reparación sin conexión (es decir, "CHKDSK/spotfix x"). |
| /perf               | Solo NTFS: (Debe usarse con "/SCAN"). Usa más recursos del sistema para completar un análisis tan rápido como sea posible. Esto puede afectar negativamente al rendimiento de otras tareas que se ejecutan en el sistema.|
| /spotfix x            | Solo NTFS: Ejecuta una corrección puntual en el volumen. |
| /sdcleanup          | Solo NTFS: La recolección de elementos no utilizados recopila datos de descriptores de seguridad innecesarios (implica/F). |
| /offlinescanandfix  | Ejecuta un examen y una corrección sin conexión en el volumen. |
| /freeorphanedchains | Solo FAT/FAT32/exFAT: Libera cualquier cadena de clúster huérfana en lugar de recuperar su contenido. |
| /markclean          | Solo FAT/FAT32/exFAT: Marca el volumen Clean si no se detectó ningún daño, aunque no se haya especificado/F. |
|         /?          | Muestra la ayuda en el símbolo del sistema.                       |

## <a name="remarks"></a>Comentarios

- Omitir comprobaciones de volumen

  El modificador **/i** o **/c** reduce la cantidad de tiempo necesario para ejecutar **CHKDSK** omitiendo ciertas comprobaciones de volumen.
- Comprobando una unidad bloqueada en el reinicio

  Si desea que **CHKDSK** corrija los errores de disco, no puede tener archivos abiertos en la unidad. Si los archivos están abiertos, aparece el siguiente mensaje de error:

  ```
  Chkdsk cannot run because the volume is in use by another process. Would you like to schedule this volume to be checked the next time the system restarts? (Y/N)  
  ``` 

  Si decide comprobar la unidad la próxima vez que reinicie el equipo, **CHKDSK** comprueba la unidad y corrige los errores automáticamente al reiniciar el equipo. Si la partición de la unidad es una partición de arranque, **CHKDSK** reinicia automáticamente el equipo después de comprobar la unidad.

  También puede usar el comando **chkntfs/c** para programar el volumen que se va a comprobar la próxima vez que se reinicie el equipo. Use el comando **fsutil Dirty Set** para establecer el bit de integridad del volumen (que indica daños), de modo que Windows Ejecute **CHKDSK** cuando se reinicie el equipo.
- Informes de errores de disco

  Debería usar **CHKDSK** en ocasiones en los sistemas de archivos FAT y NTFS para comprobar si hay errores en el disco. **CHKDSK** examina el espacio en disco y el uso del disco, y proporciona un informe de estado específico de cada sistema de archivos. El informe de estado muestra los errores encontrados en el sistema de archivos. Si ejecuta **CHKDSK** sin el parámetro **/f** en una partición activa, es posible que informe de errores falsos porque no puede bloquear la unidad.
- Corrección de errores de disco lógico

  **CHKDSK** corrige los errores de disco lógico solo si se especifica el parámetro **/f** . **CHKDSK** debe ser capaz de bloquear la unidad para corregir los errores.

  Dado que las reparaciones en sistemas de archivos FAT normalmente cambian la tabla de asignación de archivos de un disco y a veces causan una pérdida de datos, **CHKDSK** podría mostrar un mensaje de confirmación similar al siguiente:

  ```
  10 lost allocation units found in 3 chains.  
  Convert lost chains to files?  
  ``` 

  Si presiona **Y**, Windows guarda cada cadena perdida en el directorio raíz como un archivo con un nombre con el formato archivo @ no__t-1nnnn >. chk. Cuando **CHKDSK** finalice, puede comprobar estos archivos para ver si contienen los datos que necesita. Si presiona **N**, Windows corrige el disco, pero no guarda el contenido de las unidades de asignación perdidas.

  Si no usa el parámetro **/f** , **CHKDSK** muestra un mensaje que indica que es necesario corregir el archivo, pero no soluciona ningún error.

  Si usa **chkdsk/f** en un disco muy grande o en un disco con un número muy grande de archivos (por ejemplo, millones de archivos), **chkdsk/f** podría tardar mucho tiempo en completarse.

- Buscar errores de disco físico

  Use el parámetro **/r** para buscar errores de disco físico en el sistema de archivos e intentar recuperar datos de los sectores de disco afectados.

- Usar **CHKDSK** con archivos abiertos

  Si especifica el parámetro **/f** , **CHKDSK** muestra un mensaje de error si hay archivos abiertos en el disco. Si no especifica el parámetro **/f** y existen archivos abiertos, **CHKDSK** podría informar de las unidades de asignación perdidas en el disco. Esto puede ocurrir si todavía no se han grabado archivos abiertos en la tabla de asignación de archivos. Si **CHKDSK** informa de la pérdida de un gran número de unidades de asignación, considere la posibilidad de reparar el disco.

- Usar **CHKDSK** con instantáneas para carpetas compartidas

  Dado que el volumen de origen de Instantáneas para carpetas compartidas no se puede bloquear mientras Instantáneas para carpetas compartidas está habilitado, la ejecución de **CHKDSK** en el volumen de origen podría informar de errores falsos o hacer que **CHKDSK** se cierre de forma inesperada. Sin embargo, puede comprobar si hay errores en las instantáneas ejecutando **CHKDSK** en modo de solo lectura (sin parámetros) para comprobar el volumen de almacenamiento de instantáneas para carpetas compartidas.

- Descripción de los códigos de salida

  En la tabla siguiente se enumeran los códigos de salida que **CHKDSK** notifica después de que haya finalizado.  

  | Código de salida |                                                   Descripción                                                    |
  |-----------|------------------------------------------------------------------------------------------------------------------|
  |     0     |                                              No se encontraron errores.                                               |
  |     1     |                                           Se han encontrado y corregido errores.                                           |
  |     2     | Se realizó el liberador de espacio en disco (como la recolección de elementos no utilizados) o no se pudo realizar la limpieza porque no se especificó **/f** . |
  |     3     | No se pudo comprobar el disco, no se pudieron corregir los errores o no se corrigieron errores porque no se especificó **/f** .  |


- El comando **CHKDSK** , con diferentes parámetros, está disponible en la consola de recuperación.
- En los servidores que se reinician con poca frecuencia, puede que desee usar los comandos **chkntfs** o **fsutil Dirty Query** para determinar si el bit de integridad del volumen ya está establecido antes de ejecutar chkdsk.

## <a name="examples"></a>Ejemplos

Si desea comprobar el disco en la unidad D y tener errores de corrección de Windows, escriba:

```
chkdsk d: /f  
```

Si encuentra errores, **CHKDSK** pausa y muestra los mensajes. **CHKDSK** finaliza mostrando un informe que muestra el estado del disco. No se puede abrir ningún archivo en la unidad especificada hasta que finalice **CHKDSK** .

Para comprobar todos los archivos de un disco FAT en el directorio actual para los bloques no contiguos, escriba:

```
chkdsk *.*  
```

**CHKDSK** muestra un informe de estado y, a continuación, enumera los archivos que coinciden con las especificaciones de archivo que tienen bloques no contiguos.

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
