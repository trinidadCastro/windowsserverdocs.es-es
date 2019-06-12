---
title: diskcopy
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: aadb3a77cda7f1403cd2f04ced12c17617f046df
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439571"
---
# <a name="diskcopy"></a>diskcopy



Copia el contenido del disco en la unidad de origen en un disquete con o sin formato en la unidad de destino. Si se utiliza sin parámetros, **diskcopy** utiliza la unidad actual para el disco de origen y el disco de destino.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

> [!NOTE]
> Este comando no se incluye en Windows 10.

## <a name="syntax"></a>Sintaxis

```
diskcopy [<Drive1>: [<Drive2>:]] [/v]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Drive1>|Especifica la unidad que contiene el disco de origen.|
|\<Drive2>|Especifica la unidad que contiene el disco de destino.|
|/v|Comprueba que la información se copia correctamente. Esta opción se ralentiza el proceso de copia.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Uso de discos

    **Diskcopy** solo funciona con discos extraíbles, como los disquetes, que debe ser del mismo tipo. No puede usar **diskcopy** con un disco duro. Si especifica una unidad de disco duro para *unidad1* o *unidad2*, **diskcopy** muestra el mensaje de error siguiente:  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    El **diskcopy** comando le pedirá que inserte el origen y destino discos y espera a que presione cualquier tecla del teclado antes de continuar.

    Después de copia el disco, **diskcopy** muestra el mensaje siguiente:  
    ```
    Copy another diskette (Y/N)?
    ```  
    Si presiona S, **diskcopy** le pedirá que inserte los discos de origen y destino de la siguiente operación de copia. Para detener el **diskcopy** procesar, presione **N**.

    Si va a copiar en un disquete sin formato en *unidad2*, **diskcopy** formatea el disco con el mismo número de caras y sectores por pista que estén en el disco *unidad1*. **Diskcopy** muestra el siguiente mensaje mientras se formatea el disco y copia los archivos:  
    ```
    Formatting while copying
    ```  
-   Números de serie del disco

    Si el disco de origen tiene un número de serie del volumen, **diskcopy** crea un nuevo número de serie del volumen del disco de destino y muestra el número, una vez completada la operación de copia.
-   Omitiendo los parámetros de la unidad

    Si se omite el *unidad2* parámetro, **diskcopy** utiliza la unidad actual como la unidad de destino. Si omite ambos parámetros, **diskcopy** utiliza la unidad actual para ambos. Si la unidad actual es igual a *unidad1*, **diskcopy** le pedirá que cambie los discos según sea necesario.
-   Usar una única unidad para copiar

    Ejecute **diskcopy** desde una unidad distinta de la unidad de disquete, por ejemplo C de unidad. Si un disco *unidad1* y disquete *unidad2* son los mismos, **diskcopy** le pedirá que cambie los discos. Si los discos contienen más información que puede contener la memoria disponible, **diskcopy** no se puede leer toda la información a la vez. **Diskcopy** lee desde el disco de origen, escribe en el disco de destino y le pedirá que inserte el disco de origen de nuevo. Este proceso continúa hasta que haya copiado todo el disco.
-   Evitar la fragmentación de disco

    La fragmentación es la presencia de las áreas pequeñas de espacio no utilizado entre los archivos existentes en un disco. Un disco de origen fragmentado puede ralentizar el proceso de encontrar, leer o escribir archivos.

    Dado que **diskcopy** hace una copia exacta del disco de origen en el disco de destino, cualquier fragmentación del disco de origen se transfiere al disco de destino. Para evitar la transferencia de la fragmentación de un disco a otro, use **copia** o **xcopy** para copiar el disco. Dado que **copia** y **xcopy** copia los archivos de forma secuencial, el nuevo disco no está fragmentado.

> [!NOTE]
> No puede usar **xcopy** para copiar un disco de inicio.
> -   Descripción de **diskcopy** códigos de salida

    The following table explains each exit code.  
    |Código de salida|Descripción|
    |---------|-----------|
    |0|La operación de copia es correcto|
    |1|Se produjo el error recuperable de lectura/escritura|
    |3|Error grave de disco duro|
    |4|Error de inicialización|

    To process the exit codes that are returned by **diskcomp**, you can use the *ERRORLEVEL* environment variable on the **if** command line in a batch program.

## <a name="BKMK_examples"></a>Ejemplos

Para copiar el disco de la unidad B en el disco de la unidad, escriba:
```
diskcopy b: a:
```
Para usar una unidad de disco para copiar un disco a otro, primero cambie a la unidad C y, a continuación, escriba:

Diskcopy r: a:

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)