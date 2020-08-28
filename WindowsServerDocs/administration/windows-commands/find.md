---
title: find
description: Artículo de referencia para el comando Buscar, que busca una cadena de texto en archivos y muestra la cadena de texto especificada en el archivo.
ms.topic: reference
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34eb1f1cf3071147878f421307a91de921678cfc
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036543"
---
# <a name="find"></a>find

Busca una cadena de texto en un archivo o archivos y muestra líneas de texto que contienen la cadena especificada.

## <a name="syntax"></a>Sintaxis

```
find [/v] [/c] [/n] [/i] [/off[line]] <string> [[<drive>:][<path>]<filename>[...]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /v | Muestra todas las líneas que no contienen el especificado `<string>` . |
| /C | Cuenta las líneas que contienen el especificado `<string>` y muestra el total. |
| /n | Precede a cada línea con el número de línea del archivo. |
| /i | Especifica que la búsqueda no distingue entre mayúsculas y minúsculas. |
| [/OFF [línea]] | No omite los archivos que tienen el conjunto de atributos sin conexión. |
| `<string>` | Necesario. Especifica el grupo de caracteres (entre comillas) que desea buscar. |
| `[<drive>:][<path>]<filename>` | Especifica la ubicación y el nombre del archivo en el que se va a buscar la cadena especificada. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si no usa **/i**, este comando busca exactamente lo que especifica para la *cadena*. Por ejemplo, este comando trata los caracteres `a` y de `A` forma diferente. Sin embargo, si usa **/i**, la búsqueda no distingue entre mayúsculas y minúsculas y trata `a` y `A` como el mismo carácter.

- Si la cadena que desea buscar contiene comillas, debe utilizar comillas dobles para cada comilla incluida en la cadena (por ejemplo, "" esta cadena contiene comillas "").

- Si omite un nombre de archivo, este comando actúa como un filtro, tomando la entrada del origen de entrada estándar (normalmente el teclado, una barra vertical (|) o un archivo Redirigido) y, a continuación, muestra todas las líneas que contienen la *cadena*.

- Puede escribir parámetros y opciones de línea de comandos para el comando **Buscar** en cualquier orden.

- No puede usar caracteres comodín (**&#42;** y **?**) en nombres de archivo o extensiones que especifique al usar este comando. Para buscar una cadena en un conjunto de archivos que especifique con caracteres comodín, puede utilizar este comando en un comando **for** .

- Si usa **/c** y **/v** en la misma línea de comandos, este comando muestra un recuento de las líneas que no contienen la cadena especificada. Si especifica **/c** y **/n** en la misma línea de comandos, la **búsqueda** omite **/n**.

- Este comando no reconoce los retornos de carro. Cuando use este comando para buscar texto en un archivo que incluya retornos de carro, debe limitar la cadena de búsqueda al texto que se puede encontrar entre retornos de carro (es decir, una cadena que no es probable que se interrumpa con un retorno de carro). Por ejemplo, este comando no notifica una coincidencia para el archivo de impuestos de cadena si se produce un retorno de carro entre las palabras Tax y file.

### <a name="examples"></a>Ejemplos

Para mostrar todas las líneas de *Pencil.ad* que contienen el *enfoque de lápiz*de cadena, escriba:

```
find pencil sharpener pencil.ad
```

Para encontrar el texto, "los científicos etiquetaron su papel únicamente para la discusión. No es un informe final ". en el archivo de *report.doc* , escriba:

```
find ""The scientists labeled their paper for discussion only. It is not a final report."" report.doc
```

Para buscar un conjunto de archivos, puede usar el comando **Buscar** del comando **para** . Para buscar en el directorio actual los archivos que tengan la extensión. bat y que contengan la solicitud de cadena, escriba:

```
for %f in (*.bat) do find PROMPT %f
```

Para buscar en el disco duro y mostrar los nombres de archivo en la unidad C que contiene la cadena CPU, use la barra vertical (|) para dirigir la salida del comando **dir** al comando **Find** de la manera siguiente:

```
dir c:\ /s /b | find CPU
```

Dado que las búsquedas de **búsqueda** distinguen mayúsculas de minúsculas y **dir** produce una salida en mayúsculas, debe escribir la cadena CPU en mayúsculas o usar la opción de línea de comandos **/i** con **Buscar**.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando for](for.md)