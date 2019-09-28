---
title: compact
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5e73369d69912437875a0151b1d9cfcfc85a30da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379182"
---
# <a name="compact"></a>compact



Muestra o modifica la compresión de archivos o directorios en particiones NTFS. Si se usa sin parámetros, **Compact** muestra el estado de compresión del directorio actual y los archivos que contiene.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/c|Comprime el directorio o el archivo especificado.|
|/u|Descomprime el directorio o el archivo especificado.|
|/s [: \<Dir >]|Aplica el comando **Compact** a todos los subdirectorios del directorio especificado (o del directorio actual si no se especifica ninguno).|
|/a|Muestra los archivos ocultos o del sistema.|
|/i|Omite los errores.|
|/f|Fuerza la compresión o descompresión del directorio o archivo especificado. **/f** se usa en el caso de un archivo que se comprimió parcialmente cuando la operación se interrumpió por un bloqueo del sistema. Para forzar que el archivo se comprima íntegramente, use los parámetros **/c** y **/f** y especifique el archivo parcialmente comprimido.|
|/q|Solo informa de la información más esencial.|
|\<Nombre de archivo >|Especifica el archivo o directorio. Puede usar varios nombres de archivo, y **&#42;** y **?** caracteres comodín.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El comando **Compact** es la versión de línea de comandos de la característica de compresión del sistema de archivos NTFS. El estado de compresión de un directorio indica si los archivos se comprimen automáticamente cuando se agregan al directorio. Establecer el estado de compresión de un directorio no cambia necesariamente el estado de compresión de los archivos que ya están en el directorio.
-   No se puede usar **Compact** para leer, escribir o montar volúmenes comprimidos con DriveSpace o DoubleSpace.
-   No se puede usar **Compact** para comprimir particiones de tabla de asignación de archivos (FAT) o FAT32.

## <a name="BKMK_examples"></a>Example

Para establecer el estado de compresión del directorio actual, sus subdirectorios y los archivos existentes, escriba:
```
compact /c /s 
```
Para establecer el estado de compresión de los archivos y subdirectorios en el directorio actual, sin modificar el estado de compresión del propio directorio actual, escriba:
```
compact /c /s *.*
```
Para comprimir un volumen, desde el directorio raíz del volumen, escriba:
```
compact /c /i /s:\
```

> [!NOTE]
> Este ejemplo establece el estado de compresión de todos los directorios (incluido el directorio raíz en el volumen) y comprime todos los archivos del volumen. El parámetro **/i** impide que los mensajes de error interrumpan el proceso de compresión.

Para comprimir todos los archivos con la extensión de nombre de archivo. bmp en el directorio \Tmp y todos los subdirectorios de \tmp, sin modificar el atributo Compressed de los directorios, escriba:
```
compact /c /s:\tmp *.bmp
```
Para forzar la compresión completa del archivo Zebra. bmp, que se comprimió parcialmente durante un bloqueo del sistema, escriba:
```
compact /c /f zebra.bmp
```
Para quitar el atributo comprimido del directorio C:\Tmp, sin cambiar el estado de compresión de los archivos de ese directorio, escriba:
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)