---
title: dir
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8aeb2b3b7d62ae62ba9b8fa70988cf64060673ca
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79320019"
---
# <a name="dir"></a>dir



Muestra una lista de los archivos y subdirectorios de un directorio. Si se usa sin parámetros, **dir** muestra la etiqueta de volumen del disco y el número de serie, seguido de una lista de directorios y archivos del disco (incluidos sus nombres y la fecha y hora en que se modificó por última vez). En el caso de los archivos, **dir** muestra la extensión de nombre y el tamaño en bytes. **Dir** también muestra el número total de archivos y directorios enumerados, su tamaño acumulativo y el espacio libre (en bytes) restante en el disco.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
dir [<Drive>:][<Path>][<FileName>] [...] [/p] [/q] [/w] [/d] [/a[[:]<Attributes>]][/o[[:]<SortOrder>]] [/t[[:]<TimeField>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<> de unidad:] [<Path>]|Especifica la unidad y el directorio para los que desea ver una lista.|
|[\<nombre de archivo >]|Especifica un determinado archivo o grupo de archivos para los que desea ver una lista.|
|/p|Muestra una pantalla de la lista a la vez. Para ver la siguiente pantalla, presione cualquier tecla del teclado.|
|/q|Muestra información de propiedad del archivo.|
|/w|Muestra la lista en formato ancho, con un máximo de cinco nombres de archivo o de directorio en cada línea.|
|/d|Muestra la lista en el mismo formato que **/w**, pero los archivos se ordenan por columna.|
|/a [[:]\<atributos >]|Muestra solo los nombres de los directorios y archivos con los atributos que especifique. Si omite **/a**, **dir** mostrará los nombres de todos los archivos excepto los archivos ocultos y del sistema. Si usa **/a** sin especificar *atributos*, **dir** mostrará los nombres de todos los archivos, incluidos los archivos ocultos y del sistema.</br>La lista siguiente describe cada uno de los valores que puede usar para *los atributos*. Usar dos puntos (:) es opcional. Use cualquier combinación de estos valores y no separe los valores con espacios.</br>directorios **d**</br>**h** archivos ocultos</br>archivos **del sistema**</br>**l** puntos de reanálisis</br>archivos de solo lectura de **r**</br>**archivos listos** para archivar</br>**no hay** archivos indizados de contenido</br>**-** Significado del prefijo "no"|
|/o [[:]\<SortOrder >]|Ordena la salida según *SortOrder*, que puede ser cualquier combinación de los valores siguientes:</br>**n** por nombre (alfabético)</br>**e** por extensión (en orden alfabético)</br>**g** primero directorios de grupo</br>**s** por tamaño (el más pequeño primero)</br>**d** por fecha y hora (más antiguo primero)</br>**-** Prefijo para el orden inverso</br>Nota: el uso de un signo de dos puntos es opcional. Se procesan varios valores en el orden en que se enumeran. No separe varios valores con espacios.</br>Si no se especifica *SortOrder* , **dir/o** enumera los directorios en orden alfabético, seguidos de los archivos, que también se ordenan en orden alfabético.|
|/t [[:]\<TimeField >]|Especifica el campo de tiempo que se va a mostrar o usar para la ordenación. La lista siguiente describe cada uno de los valores que puede usar para *TimeField*:</br>creación de **c**</br>**último acceso**</br>**w** escritos por última vez|
|/s|Muestra todas las apariciones del nombre de archivo especificado en el directorio especificado y en todos los subdirectorios.|
|/b|Muestra una lista completa de directorios y archivos, sin información adicional. **/b** invalida **/w**.|
|/l|Muestra los nombres de directorio y los nombres de archivo sin ordenar en minúsculas.|
|/n|Muestra un formato de lista larga con nombres de archivo en el extremo derecho de la pantalla.|
|/x|Muestra los nombres cortos generados para los nombres de archivo no 8.3. La pantalla es la misma que la que se muestra para **/n**, pero el nombre corto se inserta antes del nombre largo.|
|/c|Muestra el separador de miles en los tamaños de archivo. Este es el comportamiento predeterminado. Use **/-c** para ocultar los separadores.|
|/4|Muestra los años en formato de cuatro dígitos.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Para usar varios parámetros de *nombre* de archivo, separe cada nombre de archivo con un espacio, una coma o un punto y coma.
- Puede usar caracteres comodín ( **&#42;** o **?** ) para representar uno o varios caracteres de un nombre de archivo y mostrar un subconjunto de archivos o subdirectorios.

  **Asterisco (\*):** Use el asterisco como sustituto de cualquier cadena de caracteres, por ejemplo:  
  - **dir \*. txt** enumera todos los archivos del directorio actual con extensiones que comienzan por. txt, como. txt,. txt1,. txt_old.
  - **dir read\*. txt** enumera todos los archivos del directorio actual que comienzan por "Read" y con extensiones que comienzan por. txt, como. txt,. txt1 o. txt_old.
  - **dir read\*.\*** enumera todos los archivos del directorio actual que comienzan por "Read" con cualquier extensión.

  El carácter comodín de asterisco siempre usa la asignación de nombres de archivo cortos, por lo que podría obtener resultados inesperados. Por ejemplo, el directorio siguiente contiene dos archivos (t. txt2 y T97. txt): 
 
  ```
  C:\test>dir /x
  Volume in drive C has no label.
  Volume Serial Number is B86A-EF32
    
  Directory of C:\test
    
  11/30/2004  01:40 PM <DIR>  .
  11/30/2004  01:40 PM <DIR> ..
  11/30/2004  11:05 AM 0 T97B4~1.TXT t.txt2
  11/30/2004  01:16 PM 0 t97.txt
  ```  

  Podría esperar que escribir **dir t97\\** * devolvería el archivo T97. txt. Sin embargo, al escribir **dir t97\\** * se devuelven ambos archivos, ya que el carácter comodín de asterisco coincide con el archivo t. txt2 con T97. txt mediante la asignación de nombre corto T97B4 ~ 1. txt. Del mismo modo, al escribir **del t97\\** * se eliminarían ambos archivos.

  **Signo de interrogación (?):** Use el signo de interrogación como sustituto de un solo carácter en un nombre. Por ejemplo, escribir **dir de lectura???. TXT** muestra los archivos del directorio actual con la extensión. txt que comienzan por "lectura" y van seguidos de tres caracteres como máximo. Esto incluye Read. txt, Read1. txt, Read12. txt, Read123. txt y Readme1. txt, pero no Readme12. txt.
- Especificar atributos para mostrar archivos

  Si usa **/a** con más de un valor en *los atributos*, **dir** solo mostrará los nombres de los archivos con todos los atributos especificados. Por ejemplo, si usa **/a** con **r** y **-h** como atributos (mediante **/a: r-h** o **/ar-h**), **dir** solo mostrará los nombres de los archivos de solo lectura que no estén ocultos.
- Especificar la ordenación del nombre de archivo

  Si especifica más de un valor *SortOrder* , **dir** ordena los nombres de archivo por el primer criterio, después por el segundo criterio, etc. Por ejemplo, si usa **/o** con los valores **e** y **-s** para *SortOrder* (mediante **/o: e-s** o **/OE-s**), **dir** ordena los nombres de los directorios y archivos por extensión, con el más grande primero y, a continuación, muestra el resultado final. La ordenación alfabética por extensión hace que los nombres de archivo sin extensiones aparezcan en primer lugar, los nombres de directorio y, a continuación, los nombres de archivo con extensiones.
- Usar símbolos y canalizaciones de redirección

  Cuando use el símbolo de redirección ( **>** ) para enviar la salida de la **dir** a un archivo o una canalización ( **|** ) para enviar la salida de la **dir** a otro comando, utilice **/a:-d** y **/b** para enumerar los nombres de archivo únicamente. Puede utilizar *filename* con **/b** y **/s** para especificar que **dir** debe buscar en el directorio actual y en sus subdirectorios todos los nombres de archivo que coincidan con *filename*. **Dir** muestra solo la letra de unidad, el nombre de directorio, el nombre de archivo y la extensión de nombre de archivo (una ruta de acceso por línea) para cada nombre de archivo que encuentre. Antes de usar una canalización para enviar la salida de la **dir** a otro comando, debe establecer la variable de entorno TEMP en el archivo Autoexec. NT.
- El comando **dir** , con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="examples"></a>Ejemplos

Para mostrar todos los directorios uno tras otro, en orden alfabético, en formato ancho, y en pausa después de cada pantalla, asegúrese de que el directorio raíz es el directorio actual y escriba:

```
dir /s/w/o/p
```

**Dir** muestra el directorio raíz, los subdirectorios y los archivos del directorio raíz, incluidas las extensiones. A continuación, **dir** enumera los nombres de los subdirectorios y los nombres de archivo en cada subdirectorio del árbol.

Para modificar el ejemplo anterior de modo que **dir** muestre los nombres de archivo y las extensiones, pero omite los nombres de directorio, escriba:

```
dir /s/w/o/p/a:-d
```

Para imprimir una lista de directorios, escriba:

```
dir > prn
```

Al especificar **PRN**, la lista de directorios se envía a la impresora conectada al puerto LPT1. Si la impresora está conectada a un puerto diferente, debe reemplazar **PRN** por el nombre del puerto correcto.

También puede redirigir la salida del comando **dir** a un archivo reemplazando **PRN** por un nombre de archivo. También puede escribir una ruta de acceso. Por ejemplo, para dirigir la salida de **dir** al archivo dir. doc en el directorio de registros, escriba:

```
dir > \records\dir.doc
```

Si dir. doc no existe, **dir** lo crea, a menos que el directorio Records no exista. En ese caso, aparece el siguiente mensaje:

`File creation error`

Para mostrar una lista de todos los nombres de archivo con la extensión. txt en todos los directorios de la unidad C, escriba:

```
dir c:\*.txt /w/o/s/p
```

**Dir** muestra, en formato ancho, una lista ordenada alfabéticamente de los nombres de archivo coincidentes de cada directorio y se detiene cada vez que se llena la pantalla hasta que presione cualquier tecla para continuar.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
