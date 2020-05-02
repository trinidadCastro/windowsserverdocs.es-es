---
title: compacto
description: Tema de referencia del comando Compact, que muestra o modifica la compresión de archivos o directorios en particiones NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52830530fa281025fcfd970b7675b98004e2a918
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710948"
---
# <a name="compact"></a>compacto

Muestra o modifica la compresión de archivos o directorios en particiones NTFS. Si se usa sin parámetros, **Compact** muestra el estado de compresión del directorio actual y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
compact [/c | /u] [/s[:<dir>]] [/a] [/i] [/f] [/q] [<filename>[...]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /C | Comprime el directorio o el archivo especificado. |
| /U | Descomprime el directorio o el archivo especificado. |
| /s [:`<dir>`] | Aplica el comando **Compact** a todos los subdirectorios del directorio especificado (o del directorio actual si no se especifica ninguno). |
| /a | Muestra los archivos ocultos o del sistema. |
| /i | Omite los errores. |
| /f | Fuerza la compresión o descompresión del directorio o archivo especificado. **/f** se usa en el caso de un archivo que se comprimió parcialmente cuando la operación se interrumpió por un bloqueo del sistema. Para forzar que el archivo se comprima íntegramente, use los parámetros **/c** y **/f** y especifique el archivo parcialmente comprimido. |
| /q | Solo informa de la información más esencial. |
| `<filename>` | Especifica el archivo o directorio. Puede usar varios nombres de archivo y el **&#42;** y **?** caracteres comodín. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Este comando es la versión de línea de comandos de la característica de compresión del sistema de archivos NTFS. El estado de compresión de un directorio indica si los archivos se comprimen automáticamente cuando se agregan al directorio. Establecer el estado de compresión de un directorio no cambia necesariamente el estado de compresión de los archivos que ya están en el directorio.

- No puede usar este comando para leer, escribir o montar volúmenes comprimidos con DriveSpace o DoubleSpace. Tampoco puede usar este comando para comprimir particiones de tabla de asignación de archivos (FAT) o FAT32.

## <a name="examples"></a>Ejemplos

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

Para forzar la compresión completa del archivo *Zebra. bmp*, que se comprimió parcialmente durante un bloqueo del sistema, escriba:

```
compact /c /f zebra.bmp
```

Para quitar el atributo comprimido del directorio c:\tmp, sin cambiar el estado de compresión de los archivos de ese directorio, escriba:

```
compact /u c:\tmp
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)