---
title: compacto
description: Artículo de referencia para el comando Compact, que muestra o modifica la compresión de archivos o directorios en particiones NTFS.
ms.topic: reference
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 12c835071c06255a638e65f0bdb78b0d8816cb4d
ms.sourcegitcommit: 528bdff90a7c797cdfc6839e5586f2cd5f0506b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97977250"
---
# <a name="compact"></a>compacto

Muestra o modifica la compresión de archivos o directorios en particiones NTFS. Si se usa sin parámetros, **Compact** muestra el estado de compresión del directorio actual y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
compact [/C | /U] [/S[:dir]] [/A] [/I] [/F] [/Q] [/EXE[:algorithm]] [/CompactOs[:option] [/windir:dir]] [filename [...]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Description |
| --------- | ----------- |
| /C | Comprime el directorio o el archivo especificado. Los directorios se marcan para que los archivos agregados posteriormente se compriman, a menos que se especifique el parámetro/EXE. |
| /U | Descomprime el directorio o el archivo especificado. Los directorios se marcan de modo que los archivos agregados posteriormente no se comprimen. Si se especifica el parámetro/EXE, solo se descomprimen los archivos comprimidos como ejecutables. Si no se especifica el parámetro/EXE, solo se descomprimirán los archivos NTFS comprimidos. |
| modificado`[:<dir>]` | Realiza la operación elegida en los archivos del directorio especificado y en todos los subdirectorios. De forma predeterminada, se usa el directorio actual como `<dir>` valor. |
| /a | Muestra los archivos ocultos o del sistema. De forma predeterminada, estos archivos no se incluyen. |
| /i | Continúa realizando la operación especificada y se omiten los errores. De forma predeterminada, este comando se detiene cuando se encuentra un error. |
| /f | Fuerza la compresión o descompresión del directorio o archivo especificado. Los archivos ya comprimidos se omiten de forma predeterminada. El parámetro **/f** se usa en el caso de un archivo que se comprimió parcialmente cuando la operación se interrumpió por un bloqueo del sistema. Para forzar que el archivo se comprima íntegramente, use los parámetros **/c** y **/f** y especifique el archivo parcialmente comprimido. |
| /q | Solo informa de la información más esencial. |
| /EXE | Usa la compresión optimizada para los archivos ejecutables que se leen con frecuencia, pero no se modifican. Los algoritmos admitidos son:<ul><li>**XPRESS4K** (valor predeterminado y más rápido)</li><li>**XPRESS8K**</li><li>**XPRESS16K**</li><li>**LZX** (más compacto)</li></ul> |
| /CompactOs | Establece o consulta el estado de compresión del sistema. Las opciones admitidas son:<ul><li>**query** : consulta el estado **compacto** del sistema.</li><li>**siempre** comprime todos los archivos binarios del sistema operativo y establece el estado del sistema en compacto, que permanece a menos que el administrador lo cambie.</li><li>**nunca** : descomprime todos los archivos binarios del sistema operativo y establece el estado del sistema en no compacto, que permanece a menos que el administrador lo cambie.</li></ul> |
| /WINDIR | Se usa con el parámetro **/CompactOs: query** cuando se consulta el sistema operativo sin conexión. Especifica el directorio en el que está instalado Windows. |
| `<filename>` | Especifica un patrón, un archivo o un directorio. Puede usar varios nombres de archivo y el **&#42;** y **?** caracteres comodín. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

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

Para forzar la compresión completa del archivo *zebra.bmp*, que se comprimió parcialmente durante un bloqueo del sistema, escriba:

```
compact /c /f zebra.bmp
```

Para quitar el atributo comprimido del directorio c:\tmp, sin cambiar el estado de compresión de los archivos de ese directorio, escriba:

```
compact /u c:\tmp
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)