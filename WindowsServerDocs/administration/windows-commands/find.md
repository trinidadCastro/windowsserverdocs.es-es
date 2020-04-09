---
title: find
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82b966db4117e9273ae6aed8d30baec76362de2f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844698"
---
# <a name="find"></a>find



Busca una cadena de texto en un archivo o archivos y muestra líneas de texto que contienen la cadena especificada.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
find [/v] [/c] [/n] [/i] [/off[line]] <String> [[<Drive>:][<Path>]<FileName>[...]]
```

### <a name="parameters"></a>Parámetros

|           Parámetro           |                                              Descripción                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Muestra todas las líneas que no contienen la cadena de \<especificada >.                     |
|              /c               |              Cuenta las líneas que contienen la cadena de \<especificada > y muestra el total.              |
|              /n               |                            Precede a cada línea con el número de línea del archivo.                             |
|              /i               |                            Especifica que la búsqueda no distingue entre mayúsculas y minúsculas.                            |
|         [/OFF [línea]]          |                        No omite los archivos que tienen el conjunto de atributos sin conexión.                        |
|          \<cadena >          | Obligatorio. Especifica el grupo de caracteres (entre comillas) que desea buscar. |
| [\<> de unidad:] [<Path>]<FileName> |        Especifica la ubicación y el nombre del archivo en el que se va a buscar la cadena especificada.        |
|              /?               |                                  Muestra la Ayuda en el símbolo del sistema.                                  |

## <a name="remarks"></a>Comentarios

-   Especificar una cadena

    Si no usa **/i**, **Buscar** busca exactamente lo que especifica para la *cadena*. Por ejemplo, el comando **Buscar** trata los caracteres A y a de manera diferente. Sin embargo, si usa **/i**, **Buscar** no distingue entre mayúsculas y minúsculas y trata a y a como el mismo carácter.

    Si la cadena que desea buscar contiene comillas, debe utilizar comillas dobles para cada comilla incluida en la cadena (por ejemplo, esta cadena contiene comillas).
-   Usar **Buscar** como filtro

    Si se omite un nombre de archivo, la **búsqueda** actúa como un filtro, se toma la entrada del origen de entrada estándar (normalmente, un teclado, una barra vertical (|) o un archivo Redirigido) y, a continuación, se muestran las líneas que contienen la *cadena*.
-   Sintaxis del comando de ordenación

    Puede escribir parámetros y opciones de línea de comandos para el comando **Buscar** en cualquier orden.
-   Usar caracteres comodín

    No puede usar caracteres comodín ( **&#42;** y **?** ) en nombres de archivo o extensiones que especifique con el comando **Buscar** . Para buscar una cadena en un conjunto de archivos que especifique con caracteres comodín, puede usar el comando **Buscar** en un comando **for** .
-   Usar **/v** o **/n** con **/c**

    Si usa **/c** y **/v** en la misma línea de comandos, la **búsqueda** de muestra un recuento de las líneas que no contienen la cadena especificada. Si especifica **/c** y **/n** en la misma línea de comandos, la **búsqueda** omite **/n**.
-   Usar **Buscar** con retornos de carro

    El comando **Buscar** no reconoce los retornos de carro. Cuando se usa **Buscar** para buscar texto en un archivo que incluye retornos de carro, se debe limitar la cadena de búsqueda al texto que se puede encontrar entre los retornos de carro (es decir, una cadena que no es probable que sea interrumpida por un retorno de carro). Por ejemplo, **Buscar** no notifica una coincidencia para el archivo de impuestos de cadena si se produce un retorno de carro entre las palabras Tax y file.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para mostrar todas las líneas de Pencil.ad que contienen el enfoque de lápiz de cadena, escriba:
```
find Pencil Sharpener pencil.ad
```
Para buscar una cadena que contenga texto entre comillas, debe incluir toda la cadena entre comillas. A continuación, debe usar dos comillas para cada comilla incluida dentro de la cadena. Para encontrar los científicos, etiquetó su papel únicamente para la discusión. No es un informe final. en informe. doc, escriba:
```
find The scientists labeled their paper for discussion only. It is not a final report. report.doc
```
Si desea buscar un conjunto de archivos, puede usar el comando **Buscar** del comando **para** . Para buscar en el directorio actual los archivos que tengan la extensión. bat y que contengan la solicitud de cadena, escriba:
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