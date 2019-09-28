---
title: diskcopy
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 553a85ac4fd9b7708d7adc668be4e000b36a9346
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377819"
---
# <a name="diskcopy"></a>diskcopy



Copia el contenido del disquete de la unidad de origen en un disquete formateado o sin formatear en la unidad de destino. Si se usa sin parámetros, **diskcopy** usa la unidad actual para el disco de origen y el disco de destino.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

> [!NOTE]
> Este comando no está incluido en Windows 10.

## <a name="syntax"></a>Sintaxis

```
diskcopy [<Drive1>: [<Drive2>:]] [/v]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Drive1 >|Especifica la unidad que contiene el disco de origen.|
|@no__t 0Drive2 >|Especifica la unidad que contiene el disco de destino.|
|/v|Comprueba que la información se ha copiado correctamente. Esta opción ralentiza el proceso de copia.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Uso de discos

    **Diskcopy** solo funciona con discos extraíbles, como disquetes, que deben ser del mismo tipo. No se puede usar **diskcopy** con un disco duro. Si especifica una unidad de disco duro para *unidad1* o *unidad2*, **diskcopy** mostrará el siguiente mensaje de error:  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    El comando **diskcopy** le pide que inserte los discos de origen y de destino, y espera a que presione cualquier tecla del teclado antes de continuar.

    Después de copiar el disco, **diskcopy** muestra el siguiente mensaje:  
    ```
    Copy another diskette (Y/N)?
    ```  
    Si presiona s, **diskcopy** le pedirá que inserte los discos de origen y de destino para la operación de copia siguiente. Para detener el proceso de **diskcopy** , presione **N**.

    Si va a copiar a un disquete sin formato en *unidad2*, **diskcopy** da formato al disco con el mismo número de caras y sectores por pista que el disco de la unidad de *disco.* **Diskcopy** muestra el siguiente mensaje mientras formatea el disco y copia los archivos:  
    ```
    Formatting while copying
    ```  
-   Números de serie de disco

    Si el disco de origen tiene un número de serie de volumen, **diskcopy** crea un nuevo número de serie de volumen para el disco de destino y muestra el número cuando se completa la operación de copia.
-   Omitir parámetros de unidad

    Si omite el parámetro *unidad2* , **diskcopy** usará la unidad actual como unidad de destino. Si omite ambos parámetros de unidad, **diskcopy** usará la unidad actual para ambos. Si la unidad actual es la misma que la *unidad1*, **diskcopy** le pedirá que intercambie los discos según sea necesario.
-   Usar una unidad para copiar

    Ejecute **diskcopy** desde una unidad que no sea la unidad de disquete, por ejemplo, la unidad C. Si *el disquete y la* *unidad2* del disquete son iguales, **diskcopy** le pedirá que cambie los discos. Si los discos contienen más información de la que puede contener la memoria disponible, **diskcopy** no podrá leer toda la información de una vez. **Diskcopy** lee el disco de origen, escribe en el disco de destino y le pide que vuelva a insertar el disco de origen. Este proceso continúa hasta que haya copiado todo el disco.
-   Evitar la fragmentación de disco

    La fragmentación es la presencia de pequeñas áreas de espacio en disco no utilizado entre los archivos existentes en un disco. Un disco de origen fragmentado puede ralentizar el proceso de búsqueda, lectura o escritura de archivos.

    Dado que **diskcopy** realiza una copia exacta del disco de origen en el disco de destino, cualquier fragmentación del disco de origen se transfiere al disco de destino. Para evitar la transferencia de la fragmentación de un disco a otro, utilice **Copy** o **xcopy** para copiar el disco. Dado que **Copy** y **xcopy** copian los archivos secuencialmente, el nuevo disco no se fragmenta.

> [!NOTE]
> No se puede usar **xcopy** para copiar un disco de inicio.
> -   Descripción de los códigos de salida de **diskcopy**

    The following table explains each exit code.  
    |Código de salida|Descripción|
    |---------|-----------|
    |0|La operación de copia se realizó correctamente|
    |1|Error de lectura/escritura no irrecuperable|
    |3|Se ha producido un error grave|
    |4|Error de inicialización|

    To process the exit codes that are returned by **diskcomp**, you can use the *ERRORLEVEL* environment variable on the **if** command line in a batch program.

## <a name="BKMK_examples"></a>Example

Para copiar el disco de la unidad B en el disco de la unidad A, escriba:
```
diskcopy b: a:
```
Para usar la unidad de disquete a para copiar un disquete en otro, cambie primero a la unidad C y, a continuación, escriba:

diskcopy a:

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)