---
title: forfiles
description: Artículo de referencia para el comando forfiles, que selecciona y ejecuta un comando en un archivo o conjunto de archivos.
ms.topic: reference
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/20/2020
ms.openlocfilehash: b5b2511e49c379be20c7be5abf08581a17f0a463
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634791"
---
# <a name="forfiles"></a>forfiles

Selecciona y ejecuta un comando en un archivo o conjunto de archivos. Este comando se usa normalmente en archivos por lotes.

## <a name="syntax"></a>Sintaxis

```
forfiles [/P pathname] [/M searchmask] [/S] [/C command] [/D [+ | -] [{<date> | <days>}]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /P `<pathname>` | Especifica la ruta de acceso desde la que se va a iniciar la búsqueda. De forma predeterminada, la búsqueda se inicia en el directorio de trabajo actual. |
| /M `<searchmask>` | Busca archivos según la máscara de búsqueda especificada. El valor predeterminado de searchmask es `*` . |
| /S | Indica al comando **forfiles** que busque en subdirectorios de forma recursiva. |
| /C `<command>` | Ejecuta el comando especificado en cada archivo. Las cadenas de comandos se deben incluir entre comillas dobles. El comando predeterminado es `"cmd /c echo @file"` . |
| /D. `[{+\|-}][{<date> | <days>}]` | Selecciona los archivos con una fecha de última modificación dentro del período de tiempo especificado:<ul><li>Selecciona los archivos con una fecha de última modificación posterior o igual a ( **+** ) o anterior o igual a ( **-** ) la fecha especificada, donde la *fecha* tiene el formato mm/dd/aaaa.</li><li>Selecciona los archivos con una fecha de última modificación posterior o igual a ( **+** ) la fecha actual más el número de días especificado, o anterior o igual a ( **-** ) la fecha actual menos el número de días especificado.</li><li>Los valores válidos para los *días* incluyen cualquier número en el intervalo comprendido entre 0 y 32768. Si no se especifica ningún signo, **+** se usa de forma predeterminada.</li></ul> |
| /? | Muestra el texto de ayuda en la ventana cmd. |

#### <a name="remarks"></a>Observaciones

- El `forfiles /S` comando es similar a `dir /S` .

- Puede usar las siguientes variables en la cadena de comandos tal y como se especifica en la opción de línea de comandos **/c** :

    | Variable | Descripción |
    | -------- | ----------- |
    | @FILE | Nombre de archivo. |
    | @FNAME | Nombre de archivo sin extensión. |
    | @EXT | Extensión de nombre de archivo. |
    | @PATH | Ruta de acceso completa del archivo. |
    | @RELPATH | Ruta de acceso relativa del archivo. |
    | @ISDIR | Se evalúa como TRUE si un tipo de archivo es un directorio. De lo contrario, esta variable se evalúa como FALSE. |
    | @FSIZE | Tamaño del archivo, en bytes. |
    | @FDATE | Marca de fecha de última modificación del archivo. |
    | @FTIME | Marca de tiempo de última modificación en el archivo. |

- El comando **forfiles** permite ejecutar un comando en o pasar argumentos a varios archivos. Por ejemplo, puede ejecutar el comando **Type** en todos los archivos de un árbol con la extensión de nombre de archivo. txt. O bien, puede ejecutar todos los archivos por lotes (*. bat) en la unidad C, con el nombre de archivo Myinput.txt como primer argumento.

- Este comando puede:

    - Seleccione los archivos por una fecha absoluta o relativa mediante el parámetro **/d** .

    - Cree un árbol de archivo de archivos mediante el uso de variables como @FSIZE y @FDATE .

    - Diferencie los archivos de los directorios mediante la @ISDIR variable.

    - Incluya caracteres especiales en la línea de comandos mediante el código hexadecimal del carácter, en formato 0x*HH* (por ejemplo, 0x09 para una tabulación).

- Este comando funciona implementando la `recurse subdirectories` marca en las herramientas que están diseñadas para procesar un solo archivo.

### <a name="examples"></a>Ejemplos

Para enumerar todos los archivos por lotes de la unidad C, escriba:

```
forfiles /P c:\ /S /M *.bat /C "cmd /c echo @file is a batch file"
```

Para enumerar todos los directorios de la unidad C, escriba:

```
forfiles /P c:\ /S /M *.* /C "cmd /c if @isdir==TRUE echo @file is a directory"
```

Para enumerar todos los archivos del directorio actual que tienen al menos un año, escriba:

```
forfiles /S /M *.* /D -365 /C "cmd /c echo @file is at least one year old."
```

Para mostrar el *archivo* de texto no está actualizado para cada uno de los archivos del directorio actual anteriores al 1 de enero de 2007, escriba:

```
forfiles /S /M *.* /D -01/01/2007 /C "cmd /c echo @file is outdated."
```

Para enumerar las extensiones de nombre de archivo de todos los archivos del directorio actual en formato de columna y agregar una tabulación delante de la extensión, escriba:

```
forfiles /S /M *.* /C "cmd /c echo The extension of @file is 0x09@ext"
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
