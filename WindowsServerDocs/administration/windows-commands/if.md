---
title: if
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2fafd14b0b620a8b5630c869e33c5dbc7cd902f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869236"
---
# <a name="if"></a>if



Realiza el procesamiento condicional en programas por lotes.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
if [not] ERRORLEVEL <Number> <Command> [else <Expression>]
if [not] <String1>==<String2> <Command> [else <Expression>]
if [not] exist <FileName> <Command> [else <Expression>]
```
Si se habilitan las extensiones de comando, use la sintaxis siguiente:
```
if [/i] <String1> <CompareOp> <String2> <Command> [else <Expression>]
if cmdextversion <Number> <Command> [else <Expression>]
if defined <Variable> <Command> [else <Expression>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|not|Especifica que el comando debe realizarse solo si la condición es false.|
|ERRORLEVEL \<número >|Especifica una condición true solo si el programa anterior, ejecute cmd.exe devolvió un código de salida igual o mayor que *número*.|
|\<Command>|Especifica el comando que debe llevarse a cabo si se cumple la condición anterior.|
|\<String1>==<String2>|Especifica un solo si la condición es true *String1* y *cadena2* son los mismos. Estos valores pueden ser cadenas literales o variables por lotes (por ejemplo, %1). No es necesario delimitar las cadenas literales entre comillas.|
|Existen \<nombreDeArchivo >|Especifica una condición es true si el nombre de archivo especificado no existe.|
|\<CompareOp>|Especifica un operador de comparación de tres letras. La siguiente lista representa los valores válidos para *operadordecomparación*:</br>**EQU** igual a</br>**NEQ** no es igual a</br>**LSS** menor que</br>**LEQ** menor o igual a</br>**GTR** mayor que</br>**GEQ** mayor o igual que|
|/i|Obliga a omitir mayúsculas y minúsculas en las comparaciones de cadena.  Puede usar **/i** en el *String1***==*** cadena2* forma de **si**. Estas comparaciones son genéricas, que si ambos *String1* y *cadena2* se componen de dígitos numéricos, las cadenas se convierten a números y se realiza una comparación numérica.|
|CMDEXTVERSION \<número >|Especifica el número de versión interno asociado con las extensiones de comando es igual a la característica de Cmd.exe o mayor que el número especificado de un solo si la condición es true. La primera versión es 1. Aumenta en incrementos de uno cuando se agregan mejoras significativas en las extensiones de comando. El **cmdextversion** condicional es true nunca al comando las extensiones están deshabilitadas (de forma predeterminada, se habilitan las extensiones de comando).|
|define \<Variable >|Especifica una condición es true si *Variable* está definido.|
|\<expresión >|Especifica una línea de comandos y los parámetros que se pasarán al comando en un **else** cláusula.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Si la condición especificada en una **si** cláusula es true, el comando que sigue a la condición se lleva a cabo. Si la condición es false, el comando en el **si** cláusula se omite y el comando ejecuta cualquier comando que se especifica en el **else** cláusula.
-   Cuando un programa se detiene, devuelve un código de salida. Para usar los códigos de salida como condiciones, utilice **errorlevel**.
-   Si usas **definido**, las tres variables siguientes se agregan al entorno: **% errorlevel %**, **cmdcmdline %**, y **cmdextversion %**.  
    -   **% errorlevel %** se expande en una representación de cadena del valor actual de la variable de entorno ERRORLEVEL. Esto supone que no es una variable de entorno con el nombre ERRORLEVEL, si existe, obtendrá ese valor ERRORLEVEL en su lugar.
    -   **% cmdcmdline %** se expande en la línea de comandos original que se pasó a Cmd.exe antes del procesamiento por Cmd.exe. Esto supone que no es una variable de entorno con el nombre CMDCMDLINE, si existe, se obtendrá el valor CMDCMDLINE en su lugar.
    -   **% cmdextversion %** se expande en la representación de cadena del valor actual de **cmdextversion**. Esto supone que no es una variable de entorno con el nombre CMDEXTVERSION, si existe, obtendrá su valor en su lugar.
-   Debe usar el **else** en la misma línea que el comando después de la cláusula de la **si**.

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar el mensaje "No se encuentra el archivo de datos" Si no se encuentra el archivo producto.dat, escriba:
```
if not exist product.dat echo Cannot find data file 
```
Para formatear un disco en la unidad y mostrar un mensaje de error si se produce un error durante el proceso de formato, escriba las líneas siguientes en un archivo por lotes:
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
Para eliminar el archivo producto.dat desde el directorio actual o mostrar un mensaje si no se encuentra producto.dat, escriba las líneas siguientes en un archivo por lotes:
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> Estas líneas se pueden combinar en una sola línea como sigue:
```
IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
```
Para devolver el valor de la variable de entorno ERRORLEVEL después de ejecutar un archivo por lotes, escriba las líneas siguientes del archivo por lotes:
```
goto answer%errorlevel%
:answer1
echo Program had return code 1
:answer0
echo Program had return code 0
goto end
:end
echo Done! 
```
Para ir a la etiqueta "correcta" si el valor de la variable de entorno ERRORLEVEL es menor o igual a 1, tipo:
```
if %errorlevel% LEQ 1 goto okay
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[If](if.md)

[Ir a](goto.md)