---
title: If
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e8518fffc4f271369b13899e149ebd30145726b8
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259025"
---
# <a name="if"></a>If



Realiza el procesamiento condicional en los programas por lotes.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
if [not] ERRORLEVEL <Number> <Command> [else <Expression>]
if [not] <String1>==<String2> <Command> [else <Expression>]
if [not] exist <FileName> <Command> [else <Expression>]
```
Si las extensiones de comando están habilitadas, use la siguiente sintaxis:
```
if [/i] <String1> <CompareOp> <String2> <Command> [else <Expression>]
if cmdextversion <Number> <Command> [else <Expression>]
if defined <Variable> <Command> [else <Expression>]
```

## <a name="parameters"></a>Parámetros

|        Parámetro        |                                                                                                                                                                                                                Descripción                                                                                                                                                                                                                 |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           not           |                                                                                                                                                                              Especifica que el comando solo se debe llevar a cabo si la condición es falsa.                                                                                                                                                                              |
|  ERRORLEVEL \<número >   |                                                                                                                                                      Especifica una condición verdadera solo si el programa anterior ejecutado por cmd. exe devolvió un código de salida igual o mayor que el *número*.                                                                                                                                                       |
|       \<> de comandos        |                                                                                                                                                                            Especifica el comando que debe realizarse si se cumple la condición anterior.                                                                                                                                                                             |
|  \<string1 > = =<String2>  |                                                                                                             Especifica una condición verdadera solo si *string1* y *String2* son iguales. Estos valores pueden ser cadenas literales o variables por lotes (por ejemplo, %1). No es necesario incluir cadenas literales entre comillas.                                                                                                              |
|    existe \<nombre de archivo >    |                                                                                                                                                                                       Especifica una condición verdadera si el nombre de archivo especificado existe.                                                                                                                                                                                        |
|      \<CompareOp >       |                                                                               Especifica un operador de comparación de tres letras. La lista siguiente representa los valores válidos para *CompareOp*:</br>**Equ** Igual a</br>**Neq** No es igual a</br>**LSS** Menor que</br>**Leq** Menor o igual que</br>**GTR** Mayor que</br>**GEQ** Mayor o igual que                                                                                |
|           /i            |                                                            Fuerza que las comparaciones de cadenas omitan mayúsculas y minúsculas.  Puede usar **/i** en la forma <em>cadena1</em> **==** <em>cadena2</em> de **si**. Estas comparaciones son genéricas, en el caso de que *string1* y *String2* solo consten de dígitos numéricos, las cadenas se convierten en números y se realiza una comparación numérica.                                                            |
| cmdextversion \<número > | Especifica una condición verdadera solo si el número de versión interno asociado a la característica de extensiones de comandos de cmd. exe es igual o mayor que el número especificado. La primera versión es 1. Aumenta en incrementos de uno cuando se agregan mejoras significativas a las extensiones de comando. El condicional **cmdextversion** nunca es true cuando las extensiones de comando están deshabilitadas (de forma predeterminada, las extensiones de comando están habilitadas). |
|   Variable de \<definida >   |                                                                                                                                                                                            Especifica una condición verdadera si se define la *variable* .                                                                                                                                                                                            |
|      \<expresión >      |                                                                                                                                                                   Especifica un comando de línea de comandos y los parámetros que se van a pasar al comando en una cláusula **else** .                                                                                                                                                                   |
|           /?            |                                                                                                                                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                    |

## <a name="remarks"></a>Comentarios

-   Si la condición especificada en una cláusula **If** es true, se lleva a cabo el comando que sigue a la condición. Si la condición es falsa, se omite el comando de la cláusula **If** y el comando ejecuta cualquier comando que se especifique en la cláusula **else** .
-   Cuando un programa se detiene, devuelve un código de salida. Para usar códigos de salida como condiciones, use **ERRORLEVEL**.
-   Si utiliza **definido**, se agregan las tres variables siguientes al entorno: **% ERRORLEVEL%** , **% cmdcmdline%** y **% cmdextversion%** .  
    -   **% ERRORLEVEL%** se expande en una representación de cadena del valor actual de la variable de entorno errorlevel. Se supone que no existe una variable de entorno con el nombre ERRORLEVEL; si existe, se obtendrá ese valor de ERRORLEVEL en su lugar.
    -   **% cmdcmdline%** se expande en la línea de comandos original que se pasó a cmd. exe antes de cualquier procesamiento por cmd. exe. Se supone que no existe una variable de entorno con el nombre CMDCMDLINE (si existe), obtendrá el valor CMDCMDLINE en su lugar.
    -   **% cmdextversion%** se expande en la representación de cadena del valor actual de **cmdextversion**. Se supone que no existe una variable de entorno con el nombre CMDEXTVERSION; si es así, obtendrá el valor CMDEXTVERSION en su lugar.
-   Debe usar la cláusula **else** en la misma línea que el comando después de **If**.

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar el mensaje "no se puede encontrar el archivo de datos" si no se encuentra el archivo product. dat, escriba:
```
if not exist product.dat echo Cannot find data file 
```
Para formatear un disco en la unidad A y mostrar un mensaje de error si se produce un error durante el proceso de formato, escriba las líneas siguientes en un archivo por lotes:
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
Para eliminar el archivo product. dat del directorio actual o mostrar un mensaje si no se encuentra product. dat, escriba las líneas siguientes en un archivo por lotes:
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> Estas líneas se pueden combinar en una sola línea de la siguiente manera:
> ```
> IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
> ```
> Para repetir el valor de la variable de entorno ERRORLEVEL después de ejecutar un archivo por lotes, escriba las líneas siguientes en el archivo por lotes:
> ```
> goto answer%errorlevel%
> :answer1
> echo The program returned error level 1
> goto end
> :answer0
> echo The program returned error level 0
> goto end
> :end
> echo Done! 
> ```
> Para ir a la etiqueta "correcto" si el valor de la variable de entorno ERRORLEVEL es menor o igual que 1, escriba:
> ```
> if %errorlevel% LEQ 1 goto okay
> ```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[If](if.md)

[Goto](goto.md)
