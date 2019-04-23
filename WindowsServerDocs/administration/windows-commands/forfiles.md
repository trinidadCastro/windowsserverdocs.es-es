---
title: forfiles
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 127f715620321354792d46f024ee12a06925d866
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881316"
---
# <a name="forfiles"></a>forfiles



Selecciona y ejecuta un comando en un archivo o un conjunto de archivos. Este comando es útil para el procesamiento por lotes.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c "<Command>"] [/d [{+|-}][{<Date>|<Days>}]]
```


## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/p \<ruta de acceso >|Especifica la ruta de acceso desde el que se va a iniciar la búsqueda. De forma predeterminada, la búsqueda comienza en el directorio de trabajo actual.|
|/m \<máscara de búsqueda >|Busca archivos de acuerdo con la máscara de búsqueda especificado. La máscara de búsqueda predeterminada es **\*.\***.|
|/s|Indica el **forfiles** comando para buscar en subdirectorios de forma recursiva.|
|/c "\<comando >"|Ejecuta el comando especificado en cada archivo. Cadenas de comandos deben estar entre comillas. El comando predeterminado es **"cmd /c echo @file"**.|
|/d&nbsp;[{+\|-}]&#8288;[{\<fecha >\|&#8288;\<días >}]|Selecciona archivos con una fecha de última modificación en el período de tiempo especificado.</br>: Selecciona archivos con una fecha de última modificación posterior o igual a (**+**) o anterior o igual a (**-**) la fecha especificada, donde *fecha* tiene el formato MM/DD/AAAA.</br>: Selecciona archivos con una fecha de última modificación posterior o igual a (**+**) la fecha actual más el número de días especificado, o anterior o igual a (**-**) la fecha actual menos el número de días especificado.</br>-Los valores válidos para *días* incluir cualquier número en el intervalo 0 – 32 768. Si no se especifica ningún inicio de sesión, **+** se usa de forma predeterminada.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   **Forfiles** se usa normalmente en archivos por lotes.
-   **Forfiles /s** es similar a **dir /s.**
-   Puede usar las siguientes variables en la cadena de comando según lo especificado por el **/c** opción de línea de comandos.  

|Variable|Descripción|
|--------|-----------|
|@FILE|Nombre de archivo.|
|@FNAME|Nombre de archivo sin extensión.|
|@EXT|Extensión de nombre de archivo.|
|@PATH|Ruta de acceso completa del archivo.|
|@RELPATH|Ruta de acceso relativa del archivo.|
|@ISDIR|Devuelve TRUE si un tipo de archivo es un directorio. En caso contrario, esta variable se evalúa como FALSE.|
|@FSIZE|Tamaño de archivo, en bytes.|
|@FDATE|Última marca de fecha de modificación en el archivo.|
|@FTIME|Última marca de tiempo modificada en el archivo.|

-   Con **forfiles**, puede pasar argumentos a varios archivos o ejecutar un comando en. Por ejemplo, podría ejecutar el **tipo** comando en todos los archivos en un árbol con la extensión de nombre de archivo txt. O podría ejecutar cada archivo por lotes (* .bat) en la unidad C, con el archivo de nombre "MiEntrada.txt" como primer argumento.
-   Con **forfiles**, puede realizar cualquiera de las siguientes acciones:  
    -   Seleccionar archivos por una fecha absoluta o una fecha relativa mediante la **/d** parámetro.
    -   Crear un árbol de archivos mediante las variables, como @FSIZE y @FDATE.
    -   Diferenciar los archivos de directorios mediante el uso de la @ISDIR variable.
    -   Incluir caracteres especiales en la línea de comandos con el código hexadecimal del carácter en 0 x*HH* formato (por ejemplo, 0 x 09 de una pestaña).
-   **Forfiles** funciona mediante la implementación de la **recurse subdirectorios** marca en las herramientas que están diseñadas para procesar un solo archivo.

## <a name="BKMK_examples"></a>Ejemplos

Para obtener una lista de los archivos por lotes en la unidad C, escriba:
```
forfiles /p c:\ /s /m *.bat /c "cmd /c echo @file is a batch file"
```
Para enumerar todos los directorios en la unidad C, escriba:
```
forfiles /p c:\ /s /m *.* /c "cmd /c if @isdir==TRUE echo @file is a directory"
```
Para obtener una lista de los archivos en el directorio actual que tienen una antigüedad de al menos un año, escriba:
```
forfiles /s /m *.* /d -365 /c "cmd /c echo @file is at least one year old."
```
Para mostrar el texto "*archivo* está en desuso" para cada uno de los archivos en el directorio actual que sean más antiguos que el 1 de enero de 2007, escriba:
```
forfiles /s /m *.* /d -01/01/2007 /c "cmd /c echo @file is outdated." 
```
Para enumerar las extensiones de nombre de archivo de todos los archivos en el directorio actual en formato de columna y agregar una pestaña antes de la extensión, escriba:
```
forfiles /s /m *.* /c "cmd /c echo The extension of @file is 0x09@ext" 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
