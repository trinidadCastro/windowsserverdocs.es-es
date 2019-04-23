---
title: diskcomp
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3945311873dff763a8e9e8cdd3f766c1ed4580b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837536"
---
# <a name="diskcomp"></a>diskcomp



Compara el contenido de dos discos. Si se utiliza sin parámetros, **diskcomp** utiliza la unidad actual para comparar ambos discos. Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
diskcomp [<Drive1>: [<Drive2>:]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Drive1>|Especifica la unidad que contiene uno de los discos.|
|\<Drive2>|Especifica la unidad que contiene el disco.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Uso de discos

    El **diskcomp** comando funciona solo con los discos. No puede usar **diskcomp** con un disco duro. Si especifica una unidad de disco duro para *unidad1* o *unidad2*, **diskcomp** muestra el mensaje de error siguiente:  
    ```
    Invalid drive specification
    Specified drive does not exist
    or is nonremovable
    ```  
-   Comparación de discos

    Si todas las pistas de los dos discos que se están comparadas son iguales, **diskcomp** muestra el mensaje siguiente:  
    ```
    Compare OK
    ```  
    Si las pistas no son iguales, **diskcomp** muestra un mensaje similar al siguiente:  
    ```
    Compare error on
    side 1, track 2
    ```  
    Cuando **diskcomp** finaliza la comparación, muestra el mensaje siguiente:  
    ```
    Compare another diskette (Y/N)?
    ```  
    Si presiona S, **diskcomp** le pedirá que inserte el disco para la comparación siguiente. Si presiona N, **diskcomp** detiene la comparación.

    Cuando **diskcomp** hace que la comparación, omite el número de volumen del disco.
-   Omitiendo los parámetros de la unidad

    Si se omite el *unidad2* parámetro, **diskcomp** utiliza la unidad actual para *unidad2*. Si omite ambos parámetros, **diskcomp** utiliza la unidad actual para ambos. Si la unidad actual es igual a *unidad1*, **diskcomp** le pedirá que cambie los discos según sea necesario.
-   Uso de una unidad

    Si especifica la misma unidad de disquete para *unidad1* y *unidad2*, **diskcomp** compara con una sola unidad y le pide que se va a insertar los discos según sea necesario. Es posible que deba intercambiar los discos de más de una vez, según la capacidad de los discos y la cantidad de memoria disponible.
-   Comparación de diferentes tipos de discos

    **Diskcomp** no se puede comparar un disco de una sola cara con un disco a doble cara, ni un disco con un doble densidad alta densidad. Si el disco en *unidad1* no es del mismo tipo que el disco en *unidad2*, **diskcomp** muestra el mensaje siguiente:  
    ```
    Drive types or diskette types not compatible
    ```  
-   Uso de **diskcomp** con redes y unidades redirigidas

    **Diskcomp** no funciona en una unidad de red o en una unidad creada por el **subst** comando. Si intenta usar **diskcomp** con una unidad de cualquiera de estos tipos, **diskcomp** muestra el mensaje de error siguiente:  
    ```
    Invalid drive specification
    ```  
-   Comparar un disco original con una copia

    Cuando usas **diskcomp** con un disco que se hayan realizado mediante **copia**, **diskcomp** podría mostrar un mensaje similar al siguiente:  
    ```
    Compare error on 
    side 0, track 0
    ```  
    Este tipo de error puede producirse incluso si los archivos en los discos son idénticos. Aunque **copia** duplica la información, no necesariamente colocarlo en la misma ubicación del disco de destino.
-   Descripción de **diskcomp** códigos de salida

    La siguiente tabla explica cada código de salida.  
    |Código de salida|Descripción|
    |---------|-----------|
    |0|Los discos son iguales|
    |1|Se encontraron diferencias|
    |3|Error de hardware|
    |4|Error de inicialización|

    Para procesar códigos de salida devueltos por **diskcomp**, puede usar la variable de entorno ERRORLEVEL en el **si** línea de comandos en un programa por lotes.

## <a name="BKMK_examples"></a>Ejemplos

Si el equipo tiene solo una unidad de disco (por ejemplo, la unidad A) y desea comparar dos discos, escriba:
```
diskcomp a: a:
```
**Diskcomp** le pedirá que inserte cada disco, según sea necesario.

El ejemplo siguiente muestra cómo procesar un **diskcomp** salir de código en un programa por lotes que usa la variable de entorno ERRORLEVEL en el **si** línea de comandos:
```
rem Checkout.bat compares the disks in drive A and B 
echo off 
diskcomp a: b: 
if errorlevel 4 goto ini_error 
if errorlevel 3 goto hard_error 
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok 
:ini_error 
echo ERROR: Insufficient memory or command invalid 
goto exit 
:hard_error 
echo ERROR: An irrecoverable error occurred 
goto exit 
:break 
echo "You just pressed CTRL+C" to stop the comparison 
goto exit 
:no_compare 
echo Disks are not the same 
goto exit 
:compare_ok 
echo The comparison was successful; the disks are the same 
goto exit 
:exit
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
