---
title: sort
description: Tema de referencia para la ordenación, que lee la entrada, ordena los datos y escribe los resultados en la pantalla, en un archivo o en otro dispositivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 77116469-4790-4442-8a21-9fa73b65ef9f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6eb86724a6f22f85ebad39b11a79853d0f090574
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721768"
---
# <a name="sort"></a>sort

Lee la entrada, ordena los datos y escribe los resultados en la pantalla, en un archivo o en otro dispositivo.



## <a name="syntax"></a>Sintaxis

```
sort [/r] [/+<N>] [/m <Kilobytes>] [/l <Locale>] [/rec <Characters>] [[<Drive1>:][<Path1>]<FileName1>] [/t [<Drive2>:][<Path2>]] [/o [<Drive3>:][<Path3>]<FileName3>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/r|Invierte el criterio de ordenación (es decir, ordena de Z a A y de 9 a 0).|
|/+\<N>|Especifica el número de posición del carácter donde el **orden** comenzará cada comparación. *N* puede ser cualquier entero válido.|
|/m \<kilobytes>|Especifica la cantidad de memoria principal que se va a usar para la ordenación en kilobytes (KB).|
|/l \<> de configuración regional|Invalida el criterio de ordenación de los caracteres definidos por la configuración regional predeterminada del sistema (es decir, el idioma y el país o región seleccionados durante la instalación).|
|/REC \<caracteres>|Especifica el número máximo de caracteres de un registro o una línea del archivo de entrada (el valor predeterminado es 4.096 y el máximo es 65.535).|
|[\<> unidad1:] [\<Ruta1>] \<Archivo1>|Especifica el archivo que se va a ordenar. Si no se especifica ningún nombre de archivo, se ordena la entrada estándar. La especificación del archivo de entrada es más rápida que la redirección del mismo archivo como entrada estándar.|
|/t [\<unidad2>:] [\<ruta2>]|Especifica la ruta de acceso del directorio que contiene el almacenamiento de trabajo del comando de **ordenación** si los datos no caben en la memoria principal. De forma predeterminada, se usa el directorio temporal del sistema.|
|/o [\<Drive3>:] [\<Path3>]\<FileName3>|Especifica el archivo en el que se almacenará la entrada ordenada. Si no se especifica, los datos se escriben en la salida estándar. Especificar el archivo de salida es más rápido que la redirección de la salida estándar al mismo archivo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Usar la **/+** opción de línea de comandos

    De forma predeterminada, las comparaciones se inician en el primer carácter de cada línea. La **/+** opción de línea de comandos inicia las comparaciones en el carácter especificado por *N*. Por ejemplo, `/+3` indica que cada comparación debe comenzar en el tercer carácter de cada línea. Las líneas con menos de *N* caracteres se intercalan antes que otras líneas.
-   Uso de la opción de línea de comandos **/m**

    La memoria usada siempre es un mínimo de 160 KB. Si se especifica el tamaño de memoria, se usa la cantidad exacta especificada para la ordenación (debe ser de al menos 160 KB), independientemente de la cantidad de memoria principal disponible.

    El tamaño de memoria máximo predeterminado cuando no se especifica ningún tamaño es el 90 por ciento de la memoria principal disponible si tanto la entrada como la salida son archivos, o el 45 por ciento de la memoria principal en caso contrario. La configuración predeterminada suele proporcionar el mejor rendimiento.
-   Usar la opción de línea de comandos **/l**

    Actualmente, la única alternativa a la configuración regional predeterminada es la configuración regional de C, que es más rápida que la ordenación en lenguaje natural (ordena los caracteres según sus codificaciones binarias).
-   Usar símbolos de redirección con el comando **Sort**

    Puede usar el símbolo de canalización**|**() para dirigir los datos de entrada al comando de **ordenación** desde otro comando o dirigir la salida ordenada a otro comando. Puede especificar archivos de entrada y salida mediante símbolos de redirección (**<** o **>**). Puede ser más rápido y eficaz (especialmente con archivos grandes) para especificar el archivo de entrada directamente (tal y como lo define *archivo1* en la sintaxis del comando) y, a continuación, especificar el archivo de salida con el parámetro **/o** .
-   Distinción entre mayúsculas y minúsculas

    El comando **Sort** no distingue entre mayúsculas y minúsculas.
-   Límites en el tamaño de archivo

    El comando **Sort** no tiene ningún límite en el tamaño del archivo.
-   Secuencia de intercalación

    El programa de ordenación utiliza la tabla de secuencia de intercalación que corresponde al código de país o región y a la configuración de la página de códigos. Los caracteres mayores que el código ASCII 127 se ordenan en función de la información del archivo Country. sys o de un archivo alternativo especificado por el comando **Country** en el archivo config. NT.
-   Uso de la memoria

    Si la ordenación se ajusta al tamaño máximo de memoria (como se establece de forma predeterminada o como se especifica en el parámetro **/m** ), la ordenación se realiza en un solo paso. De lo contrario, la ordenación se realiza en dos fases independientes de ordenación y combinación, y la cantidad de memoria utilizada para ambos pasos es igual. Cuando se realizan dos pasos, los datos ordenados parcialmente se almacenan en un archivo temporal en el disco. Si no hay memoria suficiente para realizar la ordenación en dos fases, se emite un error en tiempo de ejecución. Si se usa la opción de línea de comandos **/m** para especificar más memoria de la que está realmente disponible, se puede producir una degradación del rendimiento o un error en tiempo de ejecución.

## <a name="examples"></a>Ejemplos

**Ordenar un archivo**

Para ordenar y mostrar en orden inverso las líneas de un archivo denominado Expenses. txt, escriba:

`sort /r expenses.txt`

**Ordenar los resultados de un comando**

Para buscar un archivo grande llamado MailList. txt para el texto Jones y ordenar los resultados de la búsqueda, use la barra vertical (|) para dirigir la salida de un comando de **búsqueda** al comando de **ordenación** , como se indica a continuación:

`find Jones maillist.txt | sort`

El comando genera una lista ordenada de líneas que contienen el texto especificado.

**Ordenar la entrada del teclado**

Para ordenar la entrada del teclado y mostrar los resultados alfabéticamente en la pantalla, puede utilizar primero el comando de **ordenación** sin parámetros, como se indica a continuación:

`sort`

A continuación, escriba el texto que desee ordenar y presione entrar al final de cada línea. Cuando haya terminado de escribir el texto, presione CTRL + Z y, a continuación, presione Entrar. El comando **Sort** muestra el texto que ha escrito, ordenado alfabéticamente.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)