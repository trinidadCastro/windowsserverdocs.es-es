---
title: compact
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 068111b293a3eb3987b14744a1bfcf2fde26bced
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826036"
---
# <a name="compact"></a>compact



Muestra o modifica la compresión de archivos o directorios en particiones NTFS. Si se utiliza sin parámetros, **compact** muestra el estado de compresión del directorio actual y los archivos que contiene.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/c|Comprime el directorio o archivo especificado.|
|/u|Descomprime el directorio o archivo especificado.|
|/s [:\<Dir >]|Se aplica el **compact** comando a todos los subdirectorios del directorio especificado (o del directorio actual si se especifica ninguno).|
|/a|Muestra ocultado o archivos del sistema.|
|/i|Omite los errores.|
|/f|Fuerza la compresión o descompresión del archivo o directorio especificado. **/f** se usa en el caso de un archivo comprimido parcialmente debido a la operación se ha interrumpido por un bloqueo del sistema. Para forzar que el archivo se comprime en su totalidad, use el **/c** y **/f** parámetros y especifique el archivo comprimido parcialmente.|
|/q|Proporciona sólo la información esencial.|
|\<FileName>|Especifica el archivo o directorio. ¿Puede usar varios nombres de archivo y el **&#42;** y **?** caracteres comodín.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **compact** comando es la versión de línea de comandos de la característica de compresión del sistema de archivos NTFS. El estado de compresión de un directorio indica si los archivos están comprimidos automáticamente cuando se agregan al directorio. Establecer el estado de compresión de un directorio no cambia necesariamente el estado de compresión de archivos que ya están en el directorio.
-   No puede usar **compact** para lectura, escritura o montar los volúmenes que se han comprimido mediante DriveSpace o DoubleSpace.
-   No puede usar **compact** para comprimir las particiones FAT32 o una tabla de asignación de archivos (FAT).

## <a name="BKMK_examples"></a>Ejemplos

Para establecer el estado de compresión del directorio actual, sus subdirectorios y archivos existentes, escriba:
```
compact /c /s 
```
Para establecer el estado de compresión de archivos y subdirectorios dentro del directorio actual, sin alterar el estado de compresión del propio directorio actual, escriba:
```
compact /c /s *.*
```
Para comprimir un volumen desde el directorio raíz del volumen, escriba:
```
compact /c /i /s:\
```

> [!NOTE]
> Este ejemplo establece el estado de compresión de todos los directorios (incluido el directorio raíz del volumen) y comprime todos los archivos en el volumen. El **/i** parámetro impide que los mensajes de error de interrumpir el proceso de compresión.

Para comprimir todos los archivos con la extensión de nombre de archivo .bmp en el directorio \Tmp y todos los subdirectorios de \Tmp, sin modificar el atributo comprimido de los directorios, escriba:
```
compact /c /s:\tmp *.bmp
```
Para forzar la compresión completa del archivo CEBRA.bmp, parcialmente comprimido durante un bloqueo del sistema, escriba:
```
compact /c /f zebra.bmp
```
Para quitar el atributo comprimido del directorio C:\Tmp, sin cambiar el estado de compresión de los archivos en ese directorio, escriba:
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)