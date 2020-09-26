---
title: set
description: Artículo de referencia para Set, que muestra, establece o quita cmd.exe variables de entorno.
ms.topic: reference
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2f53655aa344e1770c9483e5302885734c389837
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389036"
---
# <a name="set-environment-variable"></a>Set (variable de entorno)

Muestra, establece o quita cmd.exe variables de entorno. Si se usa sin parámetros, **set** muestra la configuración actual de las variables de entorno.

> [!NOTE]
> Este comando requiere extensiones de comandos, que están habilitadas de forma predeterminada.

El comando **set** también se puede ejecutar desde la consola de recuperación de Windows, con parámetros diferentes. Para obtener más información, vea [entorno de recuperación de Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxis

```
set [<variable>=[<string>]]
set [/p] <variable>=[<promptString>]
set /a <variable>=<expression>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<variable>` | Especifica la variable de entorno que se va a establecer o modificar. |
| `<string>` | Especifica la cadena que se va a asociar a la variable de entorno especificada. |
| /p | Establece el valor de `<variable>` en una línea de entrada especificada por el usuario. |
| `<promptstring>` | Especifica un mensaje para solicitar la intervención del usuario. Este parámetro debe usarse con el parámetro **/p** . |
| /a | Establece `<string>` en una expresión numérica que se evalúa. |
| `<expression>` | Especifica una expresión numérica. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si las extensiones de comando están habilitadas (valor predeterminado) y se ejecuta **set** con un valor, se muestran todas las variables que comienzan por ese valor.

- Los caracteres `<` ,,, `>` `|` `&` y `^` son caracteres de Shell de comandos especiales y deben ir precedidos del carácter de escape ( `^` ) o encerrados entre comillas cuando se utilizan en `<string>` (por ejemplo, "StringContaining&símbolo"). Si usa comillas para incluir una cadena que contenga uno de los caracteres especiales, las comillas se establecerán como parte del valor de la variable de entorno.

- Utilice variables de entorno para controlar el comportamiento de algunos archivos y programas por lotes y para controlar la manera en que Windows y el subsistema MS-DOS aparecen y funcionan. El comando **set** se usa a menudo en el archivo **Autoexec. NT** para establecer variables de entorno.

- Si usa el comando **set** sin ningún parámetro, se muestra la configuración de entorno actual. Estos valores suelen incluir las variables de entorno **comspec** y **path** , que se usan para buscar programas en el disco. Otras dos variables de entorno usadas por Windows son **prompt** y **DIRCMD**.

- Si especifica valores para `<variable>` y `<string>` , el valor especificado `<variable>` se agrega al entorno y `<string>` se asocia a esa variable. Si la variable ya existe en el entorno, el nuevo valor de cadena reemplaza el valor de cadena anterior.

- Si especifica solo una variable y un signo igual (sin `<string>` ) para el comando **set** , `<string>` se borra el valor asociado a la variable (como si la variable no estuviera allí).

- Si usa el parámetro **/a** , se admiten los siguientes operadores, en orden descendente de prioridad:

  | Operator | Operación realizada |
  |--|--|
  | `( )` | Agrupar |
  | `! ~ -` | Unario |
  | `* / %` | Aritméticos |
  | `+ -` | Aritméticos |
  | `<< >>` | Desplazamiento lógico |
  | `&` | AND bit a bit |
  | `^` | OR exclusivo bit a bit |
  | `= *= /= %= += -= &= ^=` | `= <<= >>=` |
  | `,` | Separador de expresión |

- Si usa los operadores lógicos ( `&&` or `||` ) o modulus ( **%** ), incluya la cadena de expresión entre comillas. Las cadenas no numéricas de la expresión se consideran nombres de variable de entorno y sus valores se convierten en números antes de que se procesen. Si especifica un nombre de variable de entorno que no está definido en el entorno actual, se asigna un valor de cero, lo que le permite realizar operaciones aritméticas con valores de variables de entorno sin usar% para recuperar un valor.

- Si ejecuta **set/a** desde la línea de comandos fuera de un script de comandos, muestra el valor final de la expresión.

- Los valores numéricos son números decimales a menos que estén precedidos por 0 × para números hexadecimales o 0 para números octales. Por lo tanto, 0 × 12 es igual que 18, que es igual que 022.

- La compatibilidad con la expansión de variables de entorno diferida está deshabilitada de forma predeterminada, pero puede habilitarla o deshabilitarla mediante **cmd/v**.

- Al crear archivos por lotes, puede usar **set** para crear variables y, a continuación, utilizarlos de la misma manera que usaría las variables numeradas **%0** hasta **%9**. También puede usar las variables **%0** a **%9** como entrada para **set**.

- Si llama a un valor de variable desde un archivo por lotes, incluya el valor entre signos de porcentaje ( **%** ). Por ejemplo, si el programa por lotes crea una variable de entorno denominada *Baud*, puede usar la cadena asociada a *Baud* como parámetro reemplazable escribiendo **% Baud%** en el símbolo del sistema.

## <a name="examples"></a>Ejemplos

Para establecer una variable de entorno denominada *Test ^ 1*, escriba:

```
set testVar=test^^1
```

El comando **set** asigna todo lo que sigue al signo igual (=) al valor de la variable. Por lo tanto, si escribe `set testVar=test^1` , obtendrá el siguiente resultado: `testVar=test^1` .

Para establecer una variable de entorno denominada *TEST&1*, escriba:

```
set testVar=test^&1
```

Para establecer una variable de entorno denominada *include* para que la cadena *c:\directory* esté asociada a ella, escriba:

```
set include=c:\directory
```

Después, puede usar la cadena *c:\directory* en archivos por lotes incluyendo el nombre include con signos de porcentaje ( **%** ). Por ejemplo, puede usar `dir %include%` en un archivo por lotes para mostrar el contenido del directorio asociado a la variable de entorno include. Una vez procesado este comando, la cadena c:\directory reemplaza **% include%**.

Para usar el comando **set** en un programa por lotes para agregar un nuevo directorio a la variable de entorno *path* , escriba:

```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```

Para mostrar una lista de todas las variables de entorno que comienzan por la letra *P*, escriba:

```
set p
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)