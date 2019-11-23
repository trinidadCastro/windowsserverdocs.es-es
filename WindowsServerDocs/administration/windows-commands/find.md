---
title: find
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25cd99f3a6411c637a07b7231729cbf529a5d52e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377202"
---
# <a name="find"></a>find



Busca una cadena de texto en un archivo o archivos y muestra líneas de texto que contienen la cadena especificada.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>Parámetros

|           Parámetro           |                                              Descripción                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Muestra todas las líneas que no contienen la cadena de \<especificada >.                     |
|              /c               |              Cuenta las líneas que contienen la cadena de \<especificada > y muestra el total.              |
|              /n               |                            Precede a cada línea con el número de línea del archivo.                             |
|              /i               |                            Especifica que la búsqueda no distingue entre mayúsculas y minúsculas.                            |
|         [/OFF [línea]]          |                        No omite los archivos que tienen el conjunto de atributos sin conexión.                        |
|          "cadena de\<>"          | Obligatorio. Especifica el grupo de caracteres (entre comillas) que desea buscar. |
| [\<> de unidad:] [<Path>]<FileName> |        Especifica la ubicación y el nombre del archivo en el que se va a buscar la cadena especificada.        |
|              /?               |                                  Muestra la ayuda en el símbolo del sistema.                                  |

## <a name="remarks"></a>Observaciones

-   Especificar una cadena

    Si no usa **/i**, **Buscar** busca exactamente lo que especifica para la *cadena*. Por ejemplo, el comando **Buscar** trata los caracteres "a" y "a" de manera diferente. Sin embargo, si usa **/i**, **Buscar** no distingue entre mayúsculas y minúsculas y trata "A" y "a" como el mismo carácter.

    Si la cadena que desea buscar contiene comillas, debe utilizar comillas dobles para cada comilla de la cadena (por ejemplo, "esta" cadena "" contiene comillas ").
-   Usar **Buscar** como filtro

    Si se omite un nombre de archivo, la **búsqueda** actúa como un filtro, se toma la entrada del origen de entrada estándar (normalmente, un teclado, una barra vertical (|) o un archivo Redirigido) y, a continuación, se muestran las líneas que contienen la *cadena*.
-   Sintaxis del comando de ordenación

    Puede escribir parámetros y opciones de línea de comandos para el comando **Buscar** en cualquier orden.
-   Usar caracteres comodín

    No puede usar caracteres comodín ( **&#42;** y **?** ) en nombres de archivo o extensiones que especifique con el comando **Buscar** . Para buscar una cadena en un conjunto de archivos que especifique con caracteres comodín, puede usar el comando **Buscar** en un comando **for** .
-   Usar **/v** o **/n** con **/c**

    Si usa **/c** y **/v** en la misma línea de comandos, la **búsqueda** de muestra un recuento de las líneas que no contienen la cadena especificada. Si especifica **/c** y **/n** en la misma línea de comandos, la **búsqueda** omite **/n**.
-   Usar **Buscar** con retornos de carro

    El comando **Buscar** no reconoce los retornos de carro. Cuando se usa **Buscar** para buscar texto en un archivo que incluye retornos de carro, se debe limitar la cadena de búsqueda al texto que se puede encontrar entre los retornos de carro (es decir, una cadena que no es probable que sea interrumpida por un retorno de carro). Por ejemplo, **Buscar** no notifica una coincidencia para la cadena "archivo de impuestos" si se produce un retorno de carro entre las palabras "Tax" y "File".

## <a name="BKMK_examples"></a>Example

Para mostrar todas las líneas de Pencil.ad que contengan la cadena "lápiz Sharper", escriba:
```
find "Pencil Sharpener" pencil.ad
```
Para buscar una cadena que contenga texto entre comillas, debe incluir toda la cadena entre comillas. A continuación, debe usar dos comillas para cada comilla incluida dentro de la cadena. Para buscar "los científicos etiquetaron su papel" solo para la descripción ". No es un informe final ". en informe. doc, escriba:
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
Si desea buscar un conjunto de archivos, puede usar el comando **Buscar** del comando **para** . Para buscar en el directorio actual los archivos que tengan la extensión. bat y que contengan la cadena "PROMPT", escriba:
```
for %f in (*.bat) do find "PROMPT" %f 
```
Para buscar en el disco duro y mostrar los nombres de archivo en la unidad C que contiene la cadena "CPU", use la barra vertical (|) para dirigir la salida del comando **dir** al comando **Buscar** de la manera siguiente:
```
dir c:\ /s /b | find "CPU" 
```
Dado que las búsquedas de **búsqueda** distinguen mayúsculas de minúsculas y **dir** genera una salida en mayúsculas, debe escribir la cadena "CPU" en mayúsculas o usar la opción de línea de comandos **/i** con **Buscar**.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)