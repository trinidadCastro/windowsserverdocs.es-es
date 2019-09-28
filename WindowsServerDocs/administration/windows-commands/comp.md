---
title: comp
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 84604cea36b0b4c9543a7169002551c0da4f0493
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379258"
---
# <a name="comp"></a>comp



Compara el contenido de dos archivos o conjuntos de archivos byte a byte. Si se usa sin parámetros, **COMP** le pide que escriba los archivos que se van a comparar.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Data1 >|Especifica la ubicación y el nombre del primer archivo o conjunto de archivos que se van a comparar. Puede usar caracteres comodín ( **&#42;** y **?** ) para especificar varios archivos.|
|@no__t 0Data2 >|Especifica la ubicación y el nombre del segundo archivo o conjunto de archivos que desea comparar. Puede usar caracteres comodín ( **&#42;** y **?** ) para especificar varios archivos.|
|/d|Muestra las diferencias en el formato decimal. (El formato predeterminado es hexadecimal).|
|/a|Muestra las diferencias como caracteres.|
|/l|Muestra el número de la línea donde se produce una diferencia, en lugar de mostrar el desplazamiento de bytes.|
|/n = \<Number >|Compara solo el número de líneas que se especifican para cada archivo, incluso si los archivos tienen tamaños diferentes.|
|/c|Realiza una comparación que no distingue entre mayúsculas y minúsculas.|
|/OFF [línea]|Procesa archivos con el conjunto de atributos sin conexión.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Cómo identifica el comando **COMP** la información no coincidente

    Durante la comparación, **COMP** muestra mensajes que identifican las ubicaciones de información diferente entre los archivos. Cada mensaje indica la dirección de la memoria de desplazamiento de los bytes desiguales y el contenido de los bytes (en notación hexadecimal, a menos que se especifique el parámetro de línea de comandos **/a** o **/d** ). Los mensajes aparecen en el formato siguiente:

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    Después de diez comparaciones desiguales, **COMP** deja de comparar los archivos y muestra el siguiente mensaje:

    `10 Mismatches - ending compare`
-   Administrar casos especiales de *data1* y *Data2*  
    -   Si omite los componentes necesarios de *data1* o *Data2* o si omite *Data2*, **COMP** le solicitará la información que falta.
    -   Si *data1* solo contiene una letra de unidad o un nombre de directorio sin nombre de archivo, **COMP** compara todos los archivos del directorio especificado con el archivo especificado en *data1*.
    -   Si *Data2* solo contiene una letra de unidad o un nombre de directorio, el nombre de archivo predeterminado para *Data2* es el mismo que en *data1*.
    -   Si **COMP** no puede encontrar los archivos especificados, le pedirá un mensaje para determinar si desea comparar más archivos.
-   Comparar archivos en diferentes ubicaciones

    **COMP** puede comparar archivos en la misma unidad o en unidades diferentes, y en el mismo directorio o en directorios diferentes. Cuando **COMP** compara los archivos, muestra sus ubicaciones y nombres de archivo.
-   Comparar archivos con el mismo nombre

    Los archivos que se comparan pueden tener el mismo nombre de archivo, siempre que se encuentren en directorios diferentes o en unidades distintas. Si no especifica un nombre de archivo para *data2*, el nombre de archivo predeterminado para *data2* es el mismo que el nombre de archivo en *data1*. Puede usar caracteres comodín ( **&#42;** y **?** ) para especificar nombres de archivo.
-   Comparar archivos de distintos tamaños

    Debe especificar **/n** para comparar archivos de distintos tamaños. Si los tamaños de archivo son diferentes y no se especifica **/n** , **COMP** muestra el siguiente mensaje:

    `Files are different sizes`

    `Compare more files (Y/N)?`

    Para comparar estos archivos, presione N para detener el comando **COMP** . Después, vuelva a ejecutar el comando **COMP** con la opción **/n** para comparar solo la primera parte de cada archivo.
-   Comparar archivos secuencialmente

    Si **&#42;** usa caracteres comodín (y **?** ) para especificar varios archivos, **COMP** busca el primer archivo que coincida con *data1* y lo compara con el archivo correspondiente en *Data2*, si existe. El comando **COMP** informa de los resultados de la comparación de cada archivo que coincida con *data1*. Cuando termine, **COMP** mostrará el siguiente mensaje:

    `Compare more files (Y/N)?`

    Para comparar más archivos, presione Y. El comando **COMP** le pide las ubicaciones y los nombres de los nuevos archivos. Para detener las comparaciones, presione N. Al presionar Y, **COMP** le solicitará las opciones de línea de comandos que debe usar. Si no especifica ninguna opción de línea de comandos, **COMP** usa las que especificó antes.

## <a name="BKMK_examples"></a>Example

Para comparar el contenido del directorio C:\Informes con el directorio de copia de seguridad \\ @ no__t-1Sales\Backup\April, escriba:
```
comp c:\reports \\sales\backup\april
```
Para comparar las diez primeras líneas de los archivos de texto en el directorio \Invoice y mostrar el resultado en formato decimal, escriba:
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)