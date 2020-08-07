---
title: dir
description: Artículo de referencia para el comando dir, que muestra una lista de los archivos y subdirectorios de un directorio.
ms.topic: article
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51d36f0f5498c5c853df2d6663f52411037c13d4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890981"
---
# <a name="dir"></a>dir

Muestra una lista de los archivos y subdirectorios de un directorio. Si se usa sin parámetros, este comando muestra la etiqueta de volumen del disco y el número de serie, seguido de una lista de directorios y archivos del disco (incluidos sus nombres y la fecha y hora en que se modificó por última vez). En el caso de los archivos, este comando muestra la extensión de nombre y el tamaño en bytes. Este comando también muestra el número total de archivos y directorios enumerados, su tamaño acumulativo y el espacio libre (en bytes) restante en el disco.

El comando **dir** también se puede ejecutar desde la consola de recuperación de Windows, con parámetros diferentes. Para obtener más información, vea [entorno de recuperación de Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxis

```
dir [<drive>:][<path>][<filename>] [...] [/p] [/q] [/w] [/d] [/a[[:]<attributes>]][/o[[:]<sortorder>]] [/t[[:]<timefield>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4] [/r]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `[<drive>:][<path>]` | Especifica la unidad y el directorio para los que desea ver una lista. |
| `[<filename>]` | Especifica un determinado archivo o grupo de archivos para los que desea ver una lista. |
| /p | Muestra una pantalla de la lista a la vez. Para ver la siguiente pantalla, presione cualquier tecla. |
| /q | Muestra información de propiedad del archivo. |
| /w | Muestra la lista en formato ancho, con un máximo de cinco nombres de archivo o de directorio en cada línea. |
| /d | Muestra la lista en el mismo formato que **/w**, pero los archivos se ordenan por columna. |
| /a [[:] `<attributes>` ] | Muestra solo los nombres de los directorios y archivos con los atributos especificados. Si no usa este parámetro, el comando muestra los nombres de todos los archivos excepto los archivos ocultos y del sistema. Si usa este parámetro sin especificar ningún *atributo*, el comando muestra los nombres de todos los archivos, incluidos los archivos ocultos y del sistema. La lista de valores de *atributos* posibles es:<ul><li>directorios **d**</li><li>**h** : archivos ocultos</li><li>archivos **s** -System</li><li>**l** -puntos de análisis</li><li>**r** : archivos de solo lectura</li><li>**a** -archivos listos para archivar</li><li>no hay archivos indizados **de contenido**</li></ul>Puede usar cualquier combinación de estos valores, pero no debe separar los valores mediante espacios. Opcionalmente, puede usar dos puntos (:) separador, o puede usar un guión (-) como prefijo para indicar "no". Por ejemplo, al usar el atributo **-s** no se mostrarán los archivos del sistema. |
| /o [[:] `<sortorder>` ] | Ordena la salida según *SortOrder*, que puede ser cualquier combinación de los valores siguientes:<ul><li>**n** -alfabéticamente por nombre</li><li>**e** -alfabéticamente por extensión</li><li>**g** -grupos directorios primero</li><li>**s** -por tamaño, más pequeño primero</li><li>**d** : por fecha y hora, más antiguo primero</li><li>Usar el **-** prefijo para invertir el criterio de ordenación</li></ul>Se procesan varios valores en el orden en que se enumeran. No separe varios valores con espacios, pero puede usar opcionalmente un signo de dos puntos (:).<p>Si *SortOrder* no se especifica, **dir/o** enumera los directorios alfabéticamente, seguidos de los archivos, que también están ordenados alfabéticamente. |
| /t [[:] `<timefield>` ] | Especifica el campo de tiempo que se va a mostrar o que se va a usar para la ordenación. Los valores de *timefield* disponibles son:<ul><li>creación de **c**</li><li>**a: último** acceso</li><li>**w** : última escritura</li></ul> |
| /s | Muestra todas las apariciones del nombre de archivo especificado en el directorio especificado y en todos los subdirectorios. |
| /b | Muestra una lista completa de directorios y archivos, sin información adicional. El parámetro **/b** invalida **/w**. |
| /l | Muestra nombres de directorio y archivos no ordenados, con minúsculas. |
| /n | Muestra un formato de lista larga con nombres de archivo en el extremo derecho de la pantalla. |
| /x | Muestra los nombres cortos generados para los nombres de archivo no 8.3. La pantalla es la misma que la que se muestra para **/n**, pero el nombre corto se inserta antes del nombre largo. |
| /C | Muestra el separador de miles en los tamaños de archivo. Este es el comportamiento predeterminado. Use **/c** para ocultar los separadores. |
| /4 | Muestra los años en formato de cuatro dígitos. |
| /r | Mostrar transmisiones de datos alternativas del archivo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Para usar varios parámetros de *nombre* de archivo, separe cada nombre de archivo con un espacio, una coma o un punto y coma.

- Puede usar caracteres comodín (**&#42;** o **?**) para representar uno o varios caracteres de un nombre de archivo y mostrar un subconjunto de archivos o subdirectorios.

- Puede usar el carácter comodín, **&#42;**, para sustituir cualquier cadena de caracteres, por ejemplo:

  - `dir *.txt`enumera todos los archivos del directorio actual con extensiones que comienzan por. txt, como. txt,. txt1,. txt_old.

  - `dir read *.txt`enumera todos los archivos del directorio actual que comienzan por Read y con extensiones que comienzan por. txt, como. txt,. txt1 o. txt_old.

  - `dir read *.*`enumera todos los archivos del directorio actual que comienzan por Read con cualquier extensión.

  El carácter comodín de asterisco siempre usa la asignación de nombres de archivo cortos, por lo que podría obtener resultados inesperados. Por ejemplo, el directorio siguiente contiene dos archivos (t.txt2 y t97.txt):

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

  Podría esperar que la escritura `dir t97\*` devuelva el archivo t97.txt. Sin embargo, `dir t97\*` al escribir se devuelven ambos archivos, ya que el carácter comodín de asterisco coincide con el archivo t.txt2 para t97.txt mediante el uso de la asignación de nombre corto *T97B4 ~1.TXT*. Del mismo modo, si escribe, se `del t97\*` eliminarán ambos archivos.

- Puede usar el signo de interrogación (?) como sustituto de un solo carácter en un nombre. Por ejemplo, `dir read???.txt` al escribir se enumeran los archivos del directorio actual con la extensión. txt que comienza por Read y van seguidos de hasta tres caracteres. Esto incluye Read.txt, Read1.txt, Read12.txt, Read123.txt y Readme1.txt, pero no Readme12.txt.

- Si usa **/a** con más de un valor en *los atributos*, este comando muestra los nombres solo de los archivos con todos los atributos especificados. Por ejemplo, si usa **/a** con **r** y **-h** como atributos (mediante `/a:r-h` o `/ar-h` ), este comando solo mostrará los nombres de los archivos de solo lectura que no están ocultos.

- Si especifica más de un valor *SortOrder* , este comando ordena los nombres de archivo por el primer criterio, después por el segundo criterio, etc. Por ejemplo, si usa **/o** con los parámetros **e** y **-s** para *SortOrder* (mediante `/o:e-s` o `/oe-s` ), este comando ordena los nombres de los directorios y archivos por extensión, con el más grande primero y, a continuación, muestra el resultado final. La ordenación alfabética por extensión hace que los nombres de archivo sin extensiones aparezcan en primer lugar, los nombres de directorio y, a continuación, los nombres de archivo con extensiones.

- Si usa el símbolo de redirección ( `>` ) para enviar la salida de este comando a un archivo, o si usa una canalización ( `|` ) para enviar la salida de este comando a otro comando, debe usar `/a:-d` y **/b** para mostrar solo los nombres de archivo. Puede utilizar *filename* con **/b** y **/s** para especificar que este comando busque en el directorio actual y en sus subdirectorios todos los nombres de archivo que coincidan con *filename*. Este comando muestra solo la letra de unidad, el nombre de directorio, el nombre de archivo y la extensión de nombre de archivo (una ruta de acceso por línea) para cada nombre de archivo que encuentre. Antes de usar una canalización para enviar la salida de este comando a otro comando, debe establecer la variable de entorno *temp* en el archivo Autoexec. NT.

## <a name="examples"></a>Ejemplos

Para mostrar todos los directorios uno tras otro, en orden alfabético, en formato ancho, y en pausa después de cada pantalla, asegúrese de que el directorio raíz es el directorio actual y escriba:

```
dir /s/w/o/p
```

La salida muestra el directorio raíz, los subdirectorios y los archivos del directorio raíz, incluidas las extensiones. Este comando también muestra los nombres de los subdirectorios y los nombres de archivo en cada subdirectorio del árbol.

Para modificar el ejemplo anterior de modo que **dir** muestre los nombres de archivo y las extensiones, pero omite los nombres de directorio, escriba:

```
dir /s/w/o/p/a:-d
```

Para imprimir una lista de directorios, escriba:

```
dir > prn
```

Al especificar **PRN**, la lista de directorios se envía a la impresora conectada al puerto LPT1. Si la impresora está conectada a un puerto diferente, debe reemplazar **PRN** por el nombre del puerto correcto.

También puede redirigir la salida del comando **dir** a un archivo reemplazando **PRN** por un nombre de archivo. También puede escribir una ruta de acceso. Por ejemplo, para dirigir la salida de **dir** al archivo dir.doc en el directorio de registros, escriba:

```
dir > \records\dir.doc
```

Si dir.doc no existe, **dir** lo crea, a menos que el directorio de **registros** no exista. En ese caso, aparece el siguiente mensaje:

```
File creation error
```

Para mostrar una lista de todos los nombres de archivo con la extensión. txt en todos los directorios de la unidad C, escriba:

```
dir c:\*.txt /w/o/s/p
```

El comando **dir** muestra, en formato ancho, una lista ordenada alfabéticamente de los nombres de archivo coincidentes de cada directorio y se detiene cada vez que se llena la pantalla hasta que presione cualquier tecla para continuar.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
