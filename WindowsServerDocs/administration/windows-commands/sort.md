---
title: sort
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 77116469-4790-4442-8a21-9fa73b65ef9f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d38beef74156d9d57b947883c542c2c7e971e00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854756"
---
# <a name="sort"></a>sort



Lee la entrada, ordena datos y escribe los resultados a la pantalla, en un archivo o a otro dispositivo.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
sort [/r] [/+<N>] [/m <Kilobytes>] [/l <Locale>] [/rec <Characters>] [[<Drive1>:][<Path1>]<FileName1>] [/t [<Drive2>:][<Path2>]] [/o [<Drive3>:][<Path3>]<FileName3>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/r|Invierte el criterio de ordenación (es decir, se ordena de Z a y desde las 9 a 0).|
|/+\<N>|Especifica el número de posición del carácter donde **ordenación** se iniciará cada comparación. *N* puede ser cualquier entero válido.|
|/m \<Kilobytes>|Especifica la cantidad de memoria principal que se usará para la ordenación en kilobytes (KB).|
|/l \<locale >|Invalida el criterio de ordenación de caracteres que se definen mediante la configuración regional predeterminada del sistema (es decir, el idioma y país o región seleccionado durante la instalación).|
|/Rec \<caracteres >|Especifica el número máximo de caracteres en un registro o una línea del archivo de entrada (el valor predeterminado es 4.096 y el máximo es de 65.535).|
|[\<Drive1>:][\<Path1>]\<FileName1>|Especifica el archivo que se va a ordenar. Si no se especifica ningún nombre de archivo, se ordena la entrada estándar. Especificar el archivo de entrada es más rápido que redirigir el mismo archivo de entrada estándar.|
|/t [\<Drive2>:][\<Path2>]|Especifica la ruta de acceso del directorio que contenga el **ordenación** comando de trabajo de almacenamiento si los datos no caben en la memoria principal. De forma predeterminada, se usa el directorio temporal del sistema.|
|/o [\<Drive3>:][\<Path3>]\<FileName3>|Especifica el archivo donde se almacenará la entrada ordenada. Si no se especifica, los datos se escriben en la salida estándar. Especifica el archivo de salida es más rápido que redirija la salida estándar al mismo archivo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Mediante el **/+** opción de línea de comandos

    De forma predeterminada, las comparaciones se inician en el primer carácter de cada línea. El **/+** opción de línea de comandos inicia las comparaciones en el carácter especificado por *N*. Por ejemplo, `/+3` indica que cada comparación debería comenzar en el tercer carácter de cada línea. Líneas con menos de *N* intercalan caracteres antes de otras líneas.
-   Mediante el **/m** opción de línea de comandos

    La memoria utilizada es siempre un mínimo de 160 KB. Si se especifica el tamaño de memoria, la cantidad exacta especificada se utiliza para la ordenación (debe ser al menos 160 KB), independientemente de la cantidad de memoria está disponible.

    El tamaño máximo de memoria de forma predeterminada cuando no se especifica ningún tamaño es 90 por ciento de la memoria principal disponible si la entrada y salida de archivos o 45 por ciento de la memoria principal en caso contrario. Normalmente, la configuración predeterminada ofrece el mejor rendimiento.
-   Mediante el **/l** la opción de línea de comandos

    Actualmente, la única alternativa a la configuración regional predeterminada es la configuración regional "C", que es más rápida que la clasificación de lenguaje natural (lo ordena caracteres según su codificación binaria).
-   Usar símbolos de redirección con el **ordenación** comando

    Puede usar el símbolo de canalización (**|**) para dirigir los datos de entrada para el **ordenación** comando desde otro comando o para dirigir la salida ordenada a otro comando. Puede especificar archivos de entrada y salida mediante el uso de símbolos de redirección (**<** o **>**). Puede ser más rápido y eficaz (especialmente con archivos grandes) especificar el archivo de entrada directamente (tal como se define por *nombreDeArchivo1* en la sintaxis del comando) y, a continuación, especifique el archivo de salida mediante la **/o** parámetro.
-   Mayúsculas y minúsculas

    El **ordenación** comando no distingue entre mayúsculas y minúsculas.
-   Límites de tamaño de archivo

    El **ordenación** comando no tiene ningún límite en el tamaño de archivo.
-   Secuencia de intercalación

    El programa de ordenación usa la tabla de secuencia de intercalación que se corresponde con la configuración de código y páginas de códigos de país o región. Mayores que el código ASCII 127 caracteres se ordenan según la información contenida en el archivo Country.sys o en un archivo alternativo especificado por el **país** comando en el archivo de config.
-   Uso de memoria

    Si el criterio de ordenación se adapta el tamaño máximo de memoria (según lo establecido por el valor predeterminado o especificado por el **/m** parámetro), la ordenación se realiza en un único paso. En caso contrario, la ordenación se realiza en dos pasos independientes de ordenación y combinación y las cantidades de memoria usada para ambas fases son iguales. Cuando se realizan dos pasos, se almacenan los datos ordenados parcialmente en un archivo temporal en disco. Si no hay suficiente memoria para realizar a la ordenación en dos pasos, se emite un error en tiempo de ejecución. Si el **/m** se utiliza la opción de línea de comandos para especificar más memoria que está realmente disponible, degradación del rendimiento o puede producir un error de tiempo de ejecución.

## <a name="BKMK_examples"></a>Ejemplos

**Ordenación de un archivo**

Para ordenar y mostrar las líneas en orden inverso en un archivo denominado gastos.txt, escriba:

`sort /r expenses.txt`

**Ordenar la salida de un comando**

Para buscar un archivo grande llamado correo.txt el texto "Jones" y para ordenar los resultados de la búsqueda, use la canalización (|) para dirigir la salida de un **buscar** comando a la **ordenación** comando, como se indica a continuación:

`find "Jones" maillist.txt | sort`

El comando genera una lista ordenada de las líneas que contienen el texto especificado.

**Ordenar entradas mediante teclado**

Para ordenar la entrada de teclado y mostrar los resultados alfabéticamente en la pantalla, puede usar el **ordenación** comando sin parámetros, como se indica a continuación:

`sort`

A continuación, escriba el texto que desea ordenar y presione ENTRAR al final de cada línea. Cuando haya terminado de escribir texto, presione CTRL + Z y, a continuación, presione ENTRAR. El **ordenación** comando muestra el texto que escribió, ordenados alfabéticamente.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)