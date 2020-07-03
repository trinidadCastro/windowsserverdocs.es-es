---
title: if
description: Artículo de referencia del comando if, que realiza el procesamiento condicional en los programas por lotes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd55ebb6ae3562906efdc710f7a067a7e7514e59
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924476"
---
# <a name="if"></a>if

Realiza el procesamiento condicional en los programas por lotes.

## <a name="syntax"></a>Sintaxis

```
if [not] ERRORLEVEL <number> <command> [else <expression>]
if [not] <string1>==<string2> <command> [else <expression>]
if [not] exist <filename> <command> [else <expression>]
```

Si las extensiones de comando están habilitadas, use la siguiente sintaxis:

```
if [/i] <string1> <compareop> <string2> <command> [else <expression>]
if cmdextversion <number> <command> [else <expression>]
if defined <variable> <command> [else <expression>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| not | Especifica que el comando solo se debe llevar a cabo si la condición es falsa. |
| ERRORLEVEL`<number>` | Especifica una condición verdadera solo si el programa anterior ejecutado por Cmd.exe devolvió un código de salida igual o mayor que el *número*. |
| `<command>` | Especifica el comando que debe realizarse si se cumple la condición anterior. |
| `<string1>==<string2>` | Especifica una condición verdadera solo si *string1* y *String2* son iguales. Estos valores pueden ser cadenas literales o variables por lotes (por ejemplo, `%1` ). No es necesario incluir cadenas literales entre comillas. |
| existe`<filename>` | Especifica una condición verdadera si el nombre de archivo especificado existe. |
| `<compareop>` | Especifica un operador de comparación de tres letras, incluido:<ul><li>**Equ** : igual a</li><li>**Neq** : no es igual a</li><li>**LSS** : menor que</li><li>**Leq** : menor o igual que</li><li>**GTR** : mayor que</li><li>**GEQ** : mayor o igual que</li></ul> |
| /i | Fuerza que las comparaciones de cadenas omitan mayúsculas y minúsculas. Puede usar **/i** en el `string1==string2` formulario de **si**. Estas comparaciones son genéricas, en el caso de que *string1* y *String2* solo consten de dígitos numéricos, las cadenas se convierten en números y se realiza una comparación numérica. |
| cmdextversion`<number>` | Especifica una condición verdadera solo si el número de versión interno asociado a la característica de extensiones de comando de Cmd.exe es igual o mayor que el número especificado. La primera versión es 1. Aumenta en incrementos de uno cuando se agregan mejoras significativas a las extensiones de comando. El condicional **cmdextversion** nunca es true cuando las extensiones de comando están deshabilitadas (de forma predeterminada, las extensiones de comando están habilitadas). |
| defined `<variable>` | Especifica una condición verdadera si se define la *variable* . |
| `<expression>` | Especifica un comando de línea de comandos y los parámetros que se van a pasar al comando en una cláusula **else** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Si la condición especificada en una cláusula **If** es true, se lleva a cabo el comando que sigue a la condición. Si la condición es falsa, se omite el comando de la cláusula **If** y el comando ejecuta cualquier comando que se especifique en la cláusula **else** .

- Cuando un programa se detiene, devuelve un código de salida. Para usar códigos de salida como condiciones, use el parámetro **ERRORLEVEL** .

- Si utiliza **definido**, se agregan las tres variables siguientes al entorno: **% ERRORLEVEL%**, **% cmdcmdline%** y **% cmdextversion%**.

  - **% ERRORLEVEL%**: se expande en una representación de cadena del valor actual de la variable de entorno errorlevel. Esta variable supone que todavía no existe una variable de entorno con el nombre ERRORLEVEL. En ese caso, obtendrá ese valor de ERRORLEVEL.

  - **% cmdcmdline%**: se expande en la línea de comandos original que se pasó a Cmd.exe antes de cualquier procesamiento Cmd.exe. Se supone que ya no existe una variable de entorno con el nombre CMDCMDLINE. Si es así, obtendrá ese valor CMDCMDLINE en su lugar.

  - **% cmdextversion%**: se expande en la representación de cadena del valor actual de **cmdextversion**. Se supone que ya no existe una variable de entorno con el nombre CMDEXTVERSION. En ese caso, obtendrá ese valor de CMDEXTVERSION.

- Debe usar la cláusula **else** en la misma línea que el comando después de **If**.

### <a name="examples"></a>Ejemplos

Para mostrar el mensaje **no se encuentra el archivo de datos si no se encuentra el archivo product. dat**, escriba:

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

Para repetir el valor de la variable de entorno ERRORLEVEL después de ejecutar un archivo por lotes, escriba las líneas siguientes en el archivo por lotes:

```
goto answer%errorlevel%
:answer1
echo The program returned error level 1
goto end
:answer0
echo The program returned error level 0
goto end
:end
echo Done!
```

Para ir a la etiqueta correcto si el valor de la variable de entorno ERRORLEVEL es menor o igual que 1, escriba:

```
if %errorlevel% LEQ 1 goto okay
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [goto (comando)](goto.md)
