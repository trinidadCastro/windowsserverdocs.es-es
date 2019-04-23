---
title: chkdsk
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62912a3c-d2cc-4ef6-9679-43709a286035
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 26aad48db4a5f0a593dfcb29160031a0c9f3dc75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886866"
---
# <a name="chkdsk"></a>chkdsk



Comprueba el sistema de archivos y los metadatos del sistema de archivos de un volumen para los errores lógicos y físicos. Si se utiliza sin parámetros, **chkdsk** muestra sólo el estado del volumen y no se soluciona los errores. Si se usa con el **/f**, **/r**, **/x**, o **/b** parámetros, corrige errores en el volumen.

> [!IMPORTANT]
> Pertenecer al grupo local **administradores** , o equivalente, es lo mínimo necesario para ejecutar **chkdsk**. Para abrir una ventana del símbolo del sistema como administrador, haga clic en **símbolo** en el menú Inicio y, a continuación, haga clic en **ejecutar como administrador**.

> [!IMPORTANT]
> Interrumpir **chkdsk** no se recomienda. Sin embargo, cancelar ni interrumpir **chkdsk** no debe dejar el volumen está dañado más de lo que era antes **chkdsk** se ejecutó. Volver a ejecutar **chkdsk** comprueba y repara los daños en los restantes en el volumen.

> [!IMPORTANT]
> **Nota:** CHKDSK puede usarse solo para los discos locales. No se puede usar el comando con una letra de unidad local que se ha redirigido a través de la red.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

##<a name="syntax"></a>Sintaxis

```
chkdsk [<Volume>[[<Path>]<FileName>]] [/f] [/v] [/r] [/x] [/i] [/c] [/l[:<Size>]] [/b]  

```

##<a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Volume>|Especifica la letra de unidad (seguida de dos puntos), punto de montaje o nombre de volumen.|
|[\<Ruta de acceso >]<FileName>|Uso con la tabla de asignación de archivos (FAT) y solo FAT32. Especifica la ubicación y el nombre de un archivo o un conjunto de archivos que desea **chkdsk** para comprobar la fragmentación. ¿Puede usar el **?** y **&#42;** caracteres comodín para especificar varios archivos.|
|/f|Correcciones de errores en el disco. El disco debe estar bloqueado. Si **chkdsk** no aparece de la unidad, un mensaje de bloqueo que le pregunta si desea comprobar la unidad de la próxima vez que reinicie el equipo.|
|/v|Muestra el nombre de cada archivo en cada directorio como el disco está activado.|
|/r|Encuentra los sectores defectuosos y recupera información legible. El disco debe estar bloqueado. **/r** incluye la funcionalidad de **/f**, con el análisis adicional de errores de disco físico.|
|/x|Fuerza el volumen se desmonte en primer lugar, si es necesario. No se invalidan todos los identificadores abiertos en la unidad. **/x** también incluye la funcionalidad de **/f**.|
|/i|Solo para uso con NTFS. Realiza una comprobación menos exhaustiva de las entradas de índice, lo que reduce la cantidad de tiempo necesario para ejecutar **chkdsk**.|
|/c|Solo para uso con NTFS. No comprueba ciclos dentro de la estructura de carpetas, lo que reduce la cantidad de tiempo necesario para ejecutar **chkdsk**.|
|/l [:\<tamaño >]|Solo para uso con NTFS. Cambia el tamaño de archivo de registro para el tamaño que escriba. Si se omite el parámetro de tamaño, **/l** muestra el tamaño actual.|
|/b|Sólo para NTFS: Borra la lista de clústeres defectuosos en el volumen y vuelve a examinar todos los clústeres asignados y libres de errores. **/b** incluye la funcionalidad de **/r**. Use este parámetro después de un volumen a una nueva unidad de disco duro de creación de imágenes.|
|/?|Muestra la ayuda en el símbolo del sistema.|

##<a name="remarks"></a>Comentarios

-   Omitiendo las comprobaciones de volumen

    El **/i** o **/c** conmutador reduce la cantidad de tiempo necesario para ejecutar **chkdsk** omitiendo determinado volumen comprueba.
-   Comprobar una unidad bloqueada al reiniciar

    Si desea que **chkdsk** para corregir los errores de disco, no puede tener archivos abiertos en la unidad. Si los archivos están abiertos, aparece el mensaje de error siguiente:  
    ```
    Chkdsk cannot run because the volume is in use by another process. Would you like to schedule this volume to be checked the next time the system restarts? (Y/N)  
    
    ```  
    Si decide comprobar la unidad de la próxima vez que reinicie el equipo, **chkdsk** comprueba si la unidad y corrige automáticamente los errores cuando se reinicie el equipo. Si la unidad es una partición de arranque, **chkdsk** reinicia automáticamente el equipo después de que comprueba si la unidad.

    También puede usar el **chkntfs /c** comando para programar el volumen que se comprobará la próxima vez que se reinicie el equipo. Use la **conjunto de datos sucios fsutil** comando para establecer el volumen del bit modificado (lo que indica daños), por lo que Windows se ejecuta **chkdsk** cuando se reinicia el equipo.
-   Informes de errores de disco

    Debe usar **chkdsk** ocasionalmente en los sistemas de archivos FAT y NTFS para comprobar los errores de disco. **Chkdsk** examina espacio en disco y el uso de disco y proporciona un informe de estado específico de cada sistema de archivos. El informe de estado muestra los errores encontrados en el sistema de archivos. Si ejecuta **chkdsk** sin el **/f** parámetro en una partición activa, podría notificar falsos errores ya que no puede bloquear la unidad.
-   Corrección de errores de disco lógico

    **Chkdsk** corrige los errores de disco lógico únicamente si se especifica la **/f** parámetro. **Chkdsk** debe ser capaz de bloquear la unidad para corregir los errores.

    Dado que reparaciones en sistemas de archivos FAT cambian normalmente la tabla de asignación de archivo del disco y a veces provocar una pérdida de datos, **chkdsk** podría mostrar un mensaje de confirmación similar al siguiente:  
    ```
    10 lost allocation units found in 3 chains.  
    Convert lost chains to files?  
    
    ```  
    Si presiona **Y**, Windows guarda cada cadena perdida en el directorio raíz como un archivo con un nombre en el formato de archivo\<nnnn > .chk. Cuando **chkdsk** finalice, puede comprobar estos archivos para ver si contienen datos que necesite. Si presiona **N**, Windows corrige el disco, pero no guarda el contenido de las unidades de asignación perdidas.

    Si no usa el **/f** parámetro, **chkdsk** muestra un mensaje que el archivo debe solucionarse, pero no soluciona los errores.

    Si usas **chkdsk /f** en un disco muy grande o un disco con un gran número de archivos (por ejemplo, millones de archivos), **chkdsk /f** podría tardar mucho tiempo en completarse.
-   Buscar errores de disco físico

    Use la **/r** parámetro para buscar errores de disco físico en el sistema de archivos e intentar recuperar datos desde cualquier afectados los sectores de disco.
-   Uso de **chkdsk** con archivos abiertos

    Si especifica la **/f** parámetro, **chkdsk** muestra un mensaje de error si hay archivos abiertos en el disco. Si no especifica la **/f** parámetro y abrir archivos existe, **chkdsk** podría informar de unidades de asignación perdidas en el disco. Esto puede ocurrir si abrir archivos que no se han registrado en la tabla de asignación de archivos. Si **chkdsk** informa de la pérdida de un gran número de unidades de asignación, considere la posibilidad de reparar el disco.
-   Uso de **chkdsk** con instantáneas para carpetas compartidas

    Dado que las instantáneas de volumen de origen de carpetas compartidas no se puede bloquear mientras las instantáneas para carpetas compartidas está habilitada, ejecute **chkdsk** en el origen de volumen puede notificar errores falsos o provocar **chkdsk** se cierre inesperadamente. Sin embargo, puede comprobar instantáneas para los errores mediante la ejecución de **chkdsk** en modo de solo lectura (sin parámetros) para comprobar las instantáneas de volumen de almacenamiento de carpetas compartidas.
-   Códigos de salida de descripción

    La siguiente tabla se enumeran los códigos de la salida que **chkdsk** informes después de haber terminado.  
    |Código de salida|Descripción|
    |---------|-----------|
    |0|No se encontraron errores.|
    |1|Se encontraron errores y que se ha corregido.|
    |2|Realiza la limpieza de disco (por ejemplo, la recolección de elementos) o no realizó la limpieza porque **/f** no se ha especificado.|
    |3|No se pudo comprobar el disco, no se pudieran corregir los errores o no se han corregido errores porque **/f** no se ha especificado.|
-   El **chkdsk** comando, con diferentes parámetros, está disponible en la consola de recuperación.
-   En servidores que se reinician con poca frecuencia, es posible que desee usar el **chkntfs** o **fsutil dirty consulta** comandos para determinar si el volumen 's desfasado bits ya está establecen antes de ejecutar chkdsk.

## <a name="BKMK_examples"></a>Ejemplos

Si desea comprobar el disco en la unidad D y que corregir errores de Windows, escriba:
```
chkdsk d: /f  

```
Si se encuentran errores, **chkdsk** pone en pausa y muestra los mensajes. **Chkdsk** finaliza al mostrar un informe que muestra el estado del disco. No se puede abrir los archivos en la unidad especificada hasta **chkdsk** finaliza.

Para comprobar todos los archivos en un disco FAT en el directorio actual de bloques no contiguos, escriba:
```
chkdsk *.*  

```
**Chkdsk** muestra un informe de estado y, a continuación, enumera los archivos que coinciden con las especificaciones de archivo que tienen bloques no contiguos.
#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
