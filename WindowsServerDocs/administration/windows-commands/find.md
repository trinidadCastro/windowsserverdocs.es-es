---
title: find
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4b13a2fe573ffc81fa5c85d8fd28e9ab13ca4342
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439344"
---
# <a name="find"></a>find



Busca una cadena de texto en uno o varios archivos y muestra las líneas de texto que contienen la cadena especificada.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>Parámetros

|           Parámetro           |                                              Descripción                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Muestra todas las líneas que no tienen especificado \<String >.                     |
|              /c               |              Recuentos de las líneas que contienen el texto especificado \<String > y muestra el total.              |
|              /n               |                            Precede a cada línea con el número de línea del archivo.                             |
|              /i               |                            Especifica que la búsqueda no distingue mayúsculas de minúsculas.                            |
|         [/off[line]]          |                        No omite los archivos que tienen establecido el atributo sin conexión.                        |
|          "\<String >"          | Obligatorio. Especifica el grupo de caracteres (entre comillas) que desea buscar. |
| [\<Drive>:][<Path>]<FileName> |        Especifica la ubicación y el nombre del archivo en el que se va a buscar la cadena especificada.        |
|              /?               |                                  Muestra la ayuda en el símbolo del sistema.                                  |

## <a name="remarks"></a>Comentarios

-   Especifique una cadena

    Si no usas **/i**, **buscar** busca exactamente lo que especifique para *cadena*. Por ejemplo, el **buscar** comando trata los caracteres "a" y "A" diferente. Si usas **/i**, sin embargo, **buscar** no es distingue mayúsculas de minúsculas, y trata "a" y "A" como el mismo carácter.

    Si la cadena que desea buscar contiene comillas, debe usar comillas dobles por cada comilla contenida dentro de la cadena (por ejemplo, "este""string" "contiene comillas").
-   Uso de **buscar** como un filtro

    Si se omite un nombre de archivo, **buscar** actúa como un filtro, toma la entrada desde el origen de entrada estándar (normalmente, el teclado, una barra vertical (|) o un archivo redirigido) y, a continuación, mostrar las líneas que contengan *cadena*.
-   Sintaxis de los comandos de ordenación

    Puede escribir parámetros y las opciones de línea de comandos para el **buscar** comando en cualquier orden.
-   Uso de caracteres comodín

    No se puede usar caracteres comodín ( **&#42;** y **?** ) en los nombres de archivo o extensiones que se especifican con el **buscar** comando. Para buscar una cadena en un conjunto de archivos que se especifican con caracteres comodín, puede usar el **buscar** comando dentro de un **para** comando.
-   Uso de **/v** o **/n** con **/c**

    Si usas **/c** y **/v** en la misma línea de comandos **buscar** muestra un recuento de las líneas que no contienen la cadena especificada. Si especifica **/c** y **/n** en la misma línea de comandos **buscar** omite **/n**.
-   Uso de **buscar** con carro devuelve

    El **buscar** comando no reconoce los retornos de carro. Cuando usas **buscar** para buscar texto en un archivo que incluye los retornos de carro, debe limitar la cadena de búsqueda en texto que se encuentre entre los retornos de carro (es decir, una cadena que no es probable que se puede interrumpir por un retorno de carro). Por ejemplo, **buscar** no informa de una coincidencia para la cadena "file impuestos" si se produce un retorno de carro entre las palabras "fiscal" y "file".

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar todas las líneas del archivo escrito.AD que contengan la cadena "Lápiz Sharpener", escriba:
```
find "Pencil Sharpener" pencil.ad
```
Para buscar una cadena que contiene el texto entre comillas, debe incluir toda la cadena entre comillas. A continuación, debe usar dos comillas por cada comilla contenida dentro de la cadena. Para buscar "Los científicos etiquetados su documento"para solo discusión." No resulta un informe final." en Report.doc, escriba:
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
Si desea buscar un conjunto de archivos, puede usar el **buscar** comando dentro de la **para** comando. Para buscar el directorio actual para los archivos que tienen la extensión .bat y contienen la cadena "", escriba:
```
for %f in (*.bat) do find "PROMPT" %f 
```
Para buscar el disco duro para buscar y mostrar los nombres de archivo en la unidad C que contengan la cadena "CPU", use la canalización (|) para dirigir la salida de la **dir** comando a la **buscar** comando como sigue:
```
dir c:\ /s /b | find "CPU" 
```
Porque **buscar** las búsquedas distinguen entre mayúsculas y minúsculas y **dir** el resultado es en mayúsculas, debe escribir la cadena "CPU" en mayúsculas o usar el **/i** de línea de comandos la opción con **encontrar**.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)