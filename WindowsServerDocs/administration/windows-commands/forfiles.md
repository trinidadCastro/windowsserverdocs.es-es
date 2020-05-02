---
title: forfiles
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 21cbc24028af5c4194d36258aecdd5432fb4069f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725575"
---
# <a name="forfiles"></a>forfiles



Selecciona y ejecuta un comando en un archivo o conjunto de archivos. Este comando es útil para el procesamiento por lotes.



## <a name="syntax"></a>Sintaxis

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c <Command>] [/d [{+|-}][{<Date>|<Days>}]]
```


### <a name="parameters"></a>Parámetros

|                     Parámetro                      |                                                                                                                                                                                                                                                                                                    Descripción                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<ruta de acceso>                     |                                                                                                                                                                                                                                                 Especifica la ruta de acceso desde la que se va a iniciar la búsqueda. De forma predeterminada, la búsqueda se inicia en el directorio de trabajo actual.                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask>                  |                                                                                                                                                                                                                                                           Busca archivos según la máscara de búsqueda especificada. La máscara de búsqueda predeterminada ** \*es\\ .** \*.                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   Indica al comando **forfiles** que busque en subdirectorios de forma recursiva.                                                                                                                                                                                                                                                                    |
|                  /c \<> de comandos                   |                                                                                                                                                                                                                                  Ejecuta el comando especificado en cada archivo. Las cadenas de comandos deben ir entre comillas. El comando predeterminado es **cmd/c echo @file **.                                                                                                                                                                                                                                   |
| /d&nbsp;[{+\|-}] &#8288; [{\<Date>\|&#8288;\<días>}] | Selecciona los archivos con una fecha de última modificación dentro del período de tiempo especificado.</br>: Selecciona los archivos con una fecha de última modificación posterior o igual a**+**() o anterior o igual a (**-**) la fecha especificada, donde *fecha* tiene el formato mm/dd/aaaa.</br>: Selecciona los archivos con una fecha de última modificación posterior o igual a**+**() la fecha actual más el número de días especificado, o anterior o igual a (**-**) la fecha actual menos el número de días especificado.</br>: Los valores válidos para los *días* incluyen cualquier número en el intervalo comprendido entre 0 y 32768. Si no se especifica ningún signo **+** , se usa de forma predeterminada. |
|                         /?                         |                                                                                                                                                                                                                                                                                        Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>Observaciones

-   **Forfiles** se usa normalmente en archivos por lotes.
-   **Forfiles/s** es similar a **dir/s.**
-   Puede usar las siguientes variables en la cadena de comandos tal y como se especifica en la opción de línea de comandos **/c** .  

|Variable|Descripción|
|--------|-----------|
|@FILE|Nombre de archivo.|
|@FNAME|Nombre de archivo sin extensión.|
|@EXT|Extensión de nombre de archivo.|
|@PATH|Ruta de acceso completa del archivo.|
|@RELPATH|Ruta de acceso relativa del archivo.|
|@ISDIR|Se evalúa como TRUE si un tipo de archivo es un directorio. De lo contrario, esta variable se evalúa como FALSE.|
|@FSIZE|Tamaño del archivo, en bytes.|
|@FDATE|Marca de fecha de última modificación del archivo.|
|@FTIME|Marca de tiempo de última modificación en el archivo.|

-   Con **forfiles**, puede ejecutar un comando en o pasar argumentos a varios archivos. Por ejemplo, puede ejecutar el comando **Type** en todos los archivos de un árbol con la extensión de nombre de archivo. txt. O bien, puede ejecutar todos los archivos por lotes (*. bat) en la unidad C, con el nombre de archivo myinput. txt como primer argumento.
-   Con **forfiles**, puede realizar cualquiera de las siguientes acciones:  
    -   Seleccione los archivos por una fecha absoluta o relativa mediante el parámetro **/d** .
    -   Cree un árbol de archivo de archivos mediante el uso de @FSIZE variables @FDATEcomo y.
    -   Diferencie los archivos de los directorios mediante @ISDIR la variable.
    -   Incluya caracteres especiales en la línea de comandos mediante el código hexadecimal del carácter, en formato 0x*HH* (por ejemplo, 0x09 para una tabulación).
-   **Forfiles** funciona implementando la marca **subdirectorios de recurse** en herramientas diseñadas para procesar un solo archivo.

## <a name="examples"></a>Ejemplos

Para enumerar todos los archivos por lotes de la unidad C, escriba:
```
forfiles /p c:\ /s /m *.bat /c cmd /c echo @file is a batch file
```
Para enumerar todos los directorios de la unidad C, escriba:
```
forfiles /p c:\ /s /m *.* /c cmd /c if @isdir==TRUE echo @file is a directory
```
Para enumerar todos los archivos del directorio actual que tienen al menos un año, escriba:
```
forfiles /s /m *.* /d -365 /c cmd /c echo @file is at least one year old.
```
Para mostrar el *archivo* de texto no está actualizado para cada uno de los archivos del directorio actual anteriores al 1 de enero de 2007, escriba:
```
forfiles /s /m *.* /d -01/01/2007 /c cmd /c echo @file is outdated. 
```
Para enumerar las extensiones de nombre de archivo de todos los archivos del directorio actual en formato de columna y agregar una tabulación delante de la extensión, escriba:
```
forfiles /s /m *.* /c cmd /c echo The extension of @file is 0x09@ext 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
