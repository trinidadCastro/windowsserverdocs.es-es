---
title: findstr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82ba51cdb49501492c1fa38c6c93933f4aee90d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890466"
---
# <a name="findstr"></a>findstr



Busca patrones de texto en archivos.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/b|Coincide con el patrón de texto si está al principio de una línea.|
|/e|Coincide con el patrón de texto si está al final de una línea.|
|/l|Procesa las cadenas de búsqueda literalmente.|
|/r|Los procesos de cadenas de búsqueda como expresiones regulares. Esta es la configuración predeterminada.|
|/s|Busca en el directorio actual y todos los subdirectorios.|
|/i|Omite el caso de los caracteres al buscar la cadena.|
|/x|Imprime las líneas que coincidan exactamente.|
|/v|Imprime sólo las líneas que no contienen a una coincidencia.|
|/n|Imprime el número de línea de cada línea que coincida.|
|/m|Imprime sólo el nombre de archivo si un archivo contiene a una coincidencia.|
|/o|Imprime el desplazamiento de carácter antes de cada línea coincidente.|
|/p|Omite los archivos con los caracteres no imprimibles.|
|/off[line]|No omite los archivos que tienen establecido el atributo sin conexión.|
|/ f:\<archivo >|Obtiene una lista de archivos desde el archivo especificado.|
|/ c:\<cadena >|Utiliza el texto especificado como una cadena de búsqueda literal.|
|/ g:\<archivo >|Obtiene buscar cadenas desde el archivo especificado.|
|/ d:\<DirList >|Busca en la lista de los directorios especificados. Cada directorio debe estar separado con punto y coma (;), por ejemplo `dir1;dir2;dir3`.|
|/a:\<ColorAttribute>|Especifica atributos de color con dos dígitos hexadecimales. Tipo `color /?` para obtener más información.|
|\<Cadenas >|Especifica el texto que desea buscar en *FileName*. Obligatorio.|
|[\<Drive>:][<Path>]<FileName>[ ...]|Especifica la ubicación y el archivo o archivos que desea buscar. Se requiere al menos un nombre de archivo.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Todos los **findstr** deben preceder las opciones de línea de comandos *cadenas* y *FileName* en la cadena de comandos.
-   Expresiones regulares usan caracteres literales y metacaracteres para buscar patrones de texto, en lugar de cadenas exactas de caracteres. Un carácter literal es un carácter que no tiene un significado especial en la sintaxis de expresión regular, coincide con una aparición del carácter. Por ejemplo, letras y números son caracteres literales. Un metacarácter es un símbolo con un significado especial (un operador o un delimitador) en la sintaxis de expresión regular.

    La tabla siguiente enumeran los metacaracteres que **findstr** acepta.  
    |Metacarácter|Valor|
    |-------------|-----|
    |.|Comodín: cualquier carácter|
    |*|Repetir: cero o más apariciones del carácter anterior o la clase|
    |^|Posición de línea: a partir de la línea|
    |$|Posición de línea: final de la línea|
    |[class]|Clase de caracteres: cualquier carácter de un conjunto|
    |[^ clase]|Clase inversa: cualquier carácter no en un conjunto|
    |[x-y]|Intervalo: cualquier carácter dentro del intervalo especificado|
    |\x|Escape: uso literal de un metacarácter x|
    |\\< cadena|Posición de Word: a partir de la palabra|
    |Cadena\>|Posición de Word: final de la palabra|

    Los caracteres especiales en la sintaxis de expresiones regulares tienen la máxima potencia cuando se usan juntas. Por ejemplo, utilice la siguiente combinación del carácter comodín (.) y repita el carácter (*) para que coincida con cualquier cadena de caracteres:  
    ```
    .*
    ```  
    Utilice la siguiente expresión como parte de una expresión mayor para que coincida con cualquier cadena comienza con "b" y termina por "ing":  
    ```
    b.*ing
    ```

## <a name="BKMK_examples"></a>Ejemplos

Use espacios para separar varias cadenas de búsqueda a menos que el argumento es el prefijo **/c**.

Para buscar "hello" o "there" en el archivo x.y, escriba:
```
findstr "hello there" x.y 
```
Para buscar "Hola" en el archivo x.y, escriba:
```
findstr /c:"hello there" x.y 
```
Para buscar todas las apariciones de la palabra "Windows" (con una letra mayúscula inicial W) en el archivo propuesta.txt, escriba:
```
findstr Windows proposal.txt 
```
Para buscar todos los archivos del directorio actual y todos los subdirectorios que contienen la palabra Windows, sin tener en cuenta las mayúsculas y minúsculas, escriba:
```
findstr /s /i Windows *.* 
```
Para buscar todas las apariciones de las líneas que comienzan por "FOR" y van precedidas por cero o más espacios (como en un bucle de programa) y para mostrar el número de línea donde se encuentra cada coincidencia, escriba:
```
findstr /b /n /r /c:"^ *FOR" *.bas 
```
Para buscar varias cadenas en un conjunto de archivos, cree un archivo de texto que contiene cada criterio de búsqueda en una línea independiente. También puede enumerar los archivos exactos que desea buscar en un archivo de texto. Por ejemplo, para usar los criterios de búsqueda en el archivo Stringlist.txt, buscar los archivos enumerados en Filelist.txt y, a continuación, almacenar los resultados en resultado.out, tipo:
```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```
Para obtener una lista de todos los archivos que contienen la palabra "equipo" en el directorio actual y todos los subdirectorios, independientemente del caso, escriba:
```
findstr /s /i /m "\<computer\>" *.*
```
Para mostrar todos los archivos que contiene la palabra "equipo" y otras palabras que comienzan por "composición" (por ejemplo, "el complemento" y "competir"), escriba:
```
findstr /s /i /m "\<comp.*" *.*
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)