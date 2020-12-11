---
title: findstr
description: Artículo de referencia del comando findstr, que busca patrones de texto en archivos.
ms.topic: reference
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5fe0f2a5dfd51f3b7689db7b3ad6a38938428a01
ms.sourcegitcommit: 4165d4a9198228d4ec809ccd7d791f8de2aeb159
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97091269"
---
# <a name="findstr"></a>findstr

Busca patrones de texto en archivos.

## <a name="syntax"></a>Sintaxis

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<file>] [/c:<string>] [/g:<file>] [/d:<dirlist>] [/a:<colorattribute>] [/off[line]] <strings> [<drive>:][<path>]<filename>[ ...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /b | Coincide con el patrón de texto si está al principio de una línea. |
| /e | Coincide con el patrón de texto si está al final de una línea. |
| /l | Procesa literalmente las cadenas de búsqueda. |
| /r | Procesa las cadenas de búsqueda como expresiones regulares. Esta es la configuración predeterminada. |
| /s | Busca en el directorio actual y en todos los subdirectorios. |
| /i | Omite el uso de mayúsculas y minúsculas en los caracteres al buscar la cadena. |
| /x | Imprime las líneas que coinciden exactamente. |
| /v | Imprime solo las líneas que no contienen una coincidencia. |
| /n | Imprime el número de línea de cada línea que coincide con. |
| /m | Imprime solo el nombre de archivo si un archivo contiene una coincidencia. |
| /o | Imprime el desplazamiento de caracteres antes de cada línea coincidente. |
| /p | Omite los archivos con caracteres no imprimibles. |
| /OFF [línea] | No omite los archivos que tienen el conjunto de atributos sin conexión. |
| /f`<file>` | Obtiene una lista de archivos del archivo especificado. |
| /c`<string>` | Utiliza el texto especificado como una cadena de búsqueda literal. |
| /g`<file>` | Obtiene las cadenas de búsqueda del archivo especificado. |
| /d.`<dirlist>` | Busca la lista de directorios especificada. Cada directorio debe estar separado por un punto y coma (;), por ejemplo `dir1;dir2;dir3` . |
| /a`<colorattribute>` | Especifica los atributos de color con dos dígitos hexadecimales. Escriba `color /?` para obtener información adicional. |
| `<strings>` | Especifica el texto que se va a buscar en el *nombre de archivo*. Obligatorio. |
| `[\<drive>:][<path>]<filename>[...]` | Especifica la ubicación y el archivo o los archivos que se van a buscar. Se requiere al menos un nombre de archivo. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Todas las opciones de línea de comandos de **Findstr** deben preceder a las *cadenas* y el *nombre de archivo* en la cadena de comandos.

- Las expresiones regulares usan caracteres literales y metacaracteres para buscar patrones de texto, en lugar de cadenas exactas de caracteres.

  - Un carácter literal es un carácter que no tiene un significado especial en la sintaxis de la expresión regular; en su lugar, coincide con una aparición de ese carácter. Por ejemplo, las letras y los números son caracteres literales.

  - Un metacarácter es un símbolo con un significado especial (un operador o delimitador) en la sintaxis de la expresión regular.

    Los metacaracteres aceptados son:

    | Metacarácter | Valor |
    | -------------- | ----- |
    | `.`            | Carácter **comodín** : cualquier carácter |
    | `*`            | **REPEAT** : cero o más apariciones del carácter o la clase anterior. |
    | `^`            | **Posición de la línea inicial** : Inicio de la línea. |
    | `$`            | **Posición de línea final** : final de la línea. |
    | `[class]`      | **Clase de caracteres** : cualquier carácter de un conjunto. |
    | `[^class]`     | **Clase inversa** : cualquier carácter que no esté en un conjunto. |
    | `[x-y]`        | **Range** : cualquier carácter del intervalo especificado. |
    | `\x`           | **Escape** : uso literal de un metacarácter. |
    | `\<string`     | **Posición de la palabra inicial** : Inicio de la palabra. |
    | `string\>`     | **Posición de palabra final** de la palabra. |

    Los caracteres especiales de la sintaxis de expresiones regulares tienen la máxima eficacia cuando se usan juntos. Por ejemplo, use la combinación del carácter comodín ( `.` ) y el carácter REPEAT ( `*` ) para que coincida con cualquier cadena de caracteres: `.*`

    Use la expresión siguiente como parte de una expresión mayor para que coincida con cualquier cadena que empiece con *b* y termine con *Ing*: `b.*ing`

- Para buscar varias cadenas en un conjunto de archivos, debe crear un archivo de texto que contenga cada criterio de búsqueda en una línea independiente.

- Use espacios para separar varias cadenas de búsqueda a menos que el argumento tenga el prefijo **/c**.

### <a name="examples"></a>Ejemplos

Para buscar *Hello* *o en* el archivo *x. y*, escriba:

```
findstr hello there x.y
```

Para buscar *Hello* en el archivo *x. y*, escriba:

```
findstr /c:hello there x.y
```

Para buscar todas las apariciones de la palabra *Windows* (con una letra mayúscula inicial W) en el *proposal.txt* de archivo, escriba:

```
findstr Windows proposal.txt
```

Para buscar todos los archivos del directorio actual y todos los subdirectorios que contenían la palabra *Windows*, independientemente del caso de la letra, escriba:

```
findstr /s /i Windows *.*
```

Para buscar todas las apariciones de líneas que comienzan por *para* y van precedidas de cero o más espacios (como en un bucle de programa) y para mostrar el número de línea donde se encuentra cada repetición, escriba:

```
findstr /b /n /r /c:^ *FOR *.bas
```

Para enumerar los archivos exactos que desea buscar en un archivo de texto, use los criterios de búsqueda del archivo *stringlist.txt*, para buscar los archivos enumerados en *filelist.txt* y, a continuación, para almacenar los resultados en el archivo *Results. out*, escriba:

```
findstr /g:stringlist.txt /f:filelist.txt > results.out
```

Para enumerar todos los archivos que contengan la palabra *Computer* en el directorio actual y todos los subdirectorios, independientemente del caso, escriba:

```
findstr /s /i /m \<computer\> *.*
```

Para enumerar todos los archivos que contengan la palabra Computer y cualquier otra palabra que comience por COMP (como complemento y competir), escriba:

```
findstr /s /i /m \<comp.* *.*
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
