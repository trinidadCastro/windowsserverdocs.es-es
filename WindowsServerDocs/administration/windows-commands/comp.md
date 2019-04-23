---
title: comp
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d10b86176d97e1afd76085516fbfc00bdc36577f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854686"
---
# <a name="comp"></a>comp



Compara el contenido de dos archivos o conjuntos de archivos byte por byte. Si se utiliza sin parámetros, **comp** le pide que escriba los archivos que se compara.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Data1 >|Especifica la ubicación y el nombre del primer archivo o conjunto de archivos que desea comparar. Puede usar caracteres comodín (**&#42;** y **?**) para especificar varios archivos.|
|\<Data2 >|Especifica la ubicación y el nombre del segundo archivo o conjunto de archivos que desea comparar. Puede usar caracteres comodín (**&#42;** y **?**) para especificar varios archivos.|
|/d|Muestra las diferencias en formato decimal. (El formato predeterminado es hexadecimal).|
|/a|Muestra las diferencias como caracteres.|
|/l|Muestra el número de la línea donde se produce una diferencia, en lugar de mostrar el desplazamiento de bytes.|
|/ n =\<número >|Compara sólo el número de líneas que se especifican para cada archivo, incluso si los archivos tienen tamaños diferentes.|
|/c|Realiza una comparación que no distingue mayúsculas de minúsculas.|
|/off[line]|Procesa los archivos con el conjunto de atributos sin conexión.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El modo **comp** comando identifica información no coincidente

    Durante la comparación, **comp** muestra mensajes que identifican las ubicaciones de información desigual entre los archivos. Cada mensaje indica la dirección de memoria del desplazamiento de los bytes iguales y el contenido de los bytes (en notación hexadecimal a menos que el **/a** o **/d** se especifica el parámetro de línea de comandos). Los mensajes aparecen en el formato siguiente:

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    Después de diez comparaciones no coincidentes, **comp** detiene la comparación de los archivos y muestra el mensaje siguiente:

    `10 Mismatches - ending compare`
-   Administrar casos especiales para *Data1* y *Data2*  
    -   Si omite los componentes necesarios de uno de ellos *Data1* o *Data2* o si se omite *Data2*, **comp** le pide la información que falta.
    -   Si *Data1* contiene solo una letra de unidad o un nombre de directorio con ningún nombre de archivo, **comp** compara todos los archivos del directorio especificado en el archivo especificado en *Data1*.
    -   Si *Data2* contiene solo una letra de unidad o un nombre de directorio, el nombre de archivo predeterminado para *Data2* es igual que en *Data1*.
    -   Si **comp** no se puede encontrar los archivos que se especifica, le pedirá con un mensaje para determinar si desea comparar más archivos.
-   Comparación de archivos en ubicaciones diferentes

    **Comp** puede comparar archivos en la misma unidad en unidades diferentes y en el mismo directorio o en directorios distintos. Cuando **comp** compara los archivos, muestra sus ubicaciones y los nombres de archivo.
-   Comparar archivos con los mismos nombres

    Los archivos que se comparan pueden tener el mismo nombre de archivo, siempre que estén en directorios diferentes o en unidades distintas. Si no especifica un nombre de archivo para *Data2*, el nombre de archivo predeterminado para *Data2* es el mismo que el nombre de archivo en *Data1*. Puede usar caracteres comodín (**&#42;** y **?**) para especificar los nombres de archivo.
-   Comparar archivos de distinto tamaño

    Debe especificar **/n** para comparar archivos de diferentes tamaños. Si los tamaños de archivo son diferentes y **/n** no se especifica, **comp** muestra el mensaje siguiente:

    `Files are different sizes`

    `Compare more files (Y/N)?`

    Para comparar estos archivos, presione N para detener el **comp** comando. A continuación, vuelva a ejecutar el **comp** comando con el **/n** opción para comparar únicamente la primera parte de cada archivo.
-   Comparar archivos de forma secuencial

    Si utiliza caracteres comodín (**&#42;** y **?**) para especificar varios archivos, **comp** busca el primer archivo que coincida con *Data1* y lo compara con el archivo correspondiente en *Data2*, si existe. El **comp** comando informa de los resultados de la comparación para cada archivo que coincide con *Data1*. Cuando termine, **comp** muestra el mensaje siguiente:

    `Compare more files (Y/N)?`

    Para comparar varios archivos, presione S. El **comp** comando le pide para las ubicaciones y los nombres de los nuevos archivos. Para detener las comparaciones, presione N. Si presiona S, **comp** le pide para opciones de línea de comandos para usar. Si no especifica ninguna opción de línea de comandos, **comp** usa los que especificó antes.

## <a name="BKMK_examples"></a>Ejemplos

Para comparar el contenido del directorio C:\Reports con el directorio de copia de seguridad \\ \\Sales\Backup\April, tipo:
```
comp c:\reports \\sales\backup\april
```
Para comparar las diez primeras líneas de los archivos de texto en el directorio \Invoice y mostrar el resultado en formato decimal, escriba:
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)