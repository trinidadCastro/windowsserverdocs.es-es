---
title: set
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12dce38bf8ad050c65a7a8c0fca4a71267cca93f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384105"
---
# <a name="set"></a>set



Muestra, establece o quita CMD. Variables de entorno de EXE. Si se usa sin parámetros, **set** muestra la configuración actual de las variables de entorno.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
set [<Variable>=[<String>]]
set [/p] <Variable>=[<PromptString>]
set /a <Variable>=<Expression>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Variable >|Especifica la variable de entorno que se va a establecer o modificar.|
|@no__t 0String >|Especifica la cadena que se va a asociar a la variable de entorno especificada.|
|/p|Establece el valor de la *variable* en una línea de entrada especificada por el usuario.|
|@no__t 0PromptString >|Opcional. Especifica un mensaje para solicitar la intervención del usuario. Este parámetro se usa con la opción de línea de comandos **/p** .|
|/a|Establece una *cadena* en una expresión numérica que se evalúa.|
|@no__t 0Expression >|Especifica una expresión numérica. Vea la sección Comentarios para ver los operadores válidos que se pueden usar en la *expresión*.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Usar **set** con extensiones de comandos habilitadas

  Cuando se habilitan las extensiones de comando (valor predeterminado) y se ejecuta **set** con un valor, se muestran todas las variables que comienzan por ese valor.
- Usar caracteres especiales

  Los caracteres **<** , **>** , **|** , **&** , **^** son caracteres de Shell de comandos especiales y deben ir precedidos del carácter de escape (**1**) o encerrarse entre comillas. Cuando se usa en una *cadena* (por ejemplo, **"StringContaining & Symbol"** ). Si utiliza comillas para incluir una cadena que contenga uno de los caracteres especiales, las comillas se establecerán como parte del valor de la variable de entorno.
- Usar variables de entorno

  Utilice variables de entorno para controlar el comportamiento de algunos archivos y programas por lotes y para controlar la manera en que Windows y el subsistema MS-DOS aparecen y funcionan. El comando **set** se usa a menudo en el archivo Autoexec. NT para establecer variables de entorno.
- Mostrar la configuración del entorno actual

  Al escribir el comando **set** por sí solo, se muestra la configuración del entorno actual. Estos valores suelen incluir las variables de entorno comspec y PATH, que se usan para buscar programas en el disco. Otras dos variables de entorno usadas por Windows son PROMPT y DIRCMD.
- Usar parámetros

  Cuando se especifican valores para *variable* y *cadena*, el valor de la *variable* especificada se agrega al entorno y la *cadena* se asocia a esa variable. Si la variable ya existe en el entorno, el nuevo valor de cadena reemplaza el valor de cadena anterior.

  Si especifica solo una variable y un signo igual (sin *cadena*) para el comando **set** , se borra el valor de *cadena* asociado a la variable (como si la variable no estuviera allí).
- Usar **/a**

  En la tabla siguiente se enumeran los operadores admitidos para **/a** en orden descendente de prioridad.  

  |        Operador         | Operación realizada  |
  |-------------------------|----------------------|
  |           ( )           |       Agrupar       |
  |          ! ~ -          |        Unario         |
  |         \*/%          |      Operador      |
  |           + -           |      Operador      |
  |          < < > >          |    Desplazamiento lógico     |
  |            &            |     And bit a bit      |
  |            ^            | Or exclusivo bit a bit |
  |                         |                      |
  | =  @ no__t-1 =/=% = + =-= & = ^ = |      = < < = > > =       |
  |            ,            | Separador de expresión |

  Si usa operadores lógicos ( **&&** o **||** ) o módulo ( **%** ), incluya la cadena de expresión entre comillas. Las cadenas no numéricas de la expresión se consideran nombres de variable de entorno y sus valores se convierten en números antes de que se procesen. Si especifica un nombre de variable de entorno que no está definido en el entorno actual, se asigna un valor de cero, lo que le permite realizar operaciones aritméticas con valores de variables de entorno sin usar% para recuperar un valor.

  Si ejecuta **set/a** desde la línea de comandos fuera de un script de comandos, muestra el valor final de la expresión.

  Los valores numéricos son números decimales a menos que estén precedidos por 0 × para números hexadecimales o 0 para números octales. Por lo tanto, 0 × 12 es igual que 18, que es igual que 022.
- Compatibilidad con la expansión de variables de entorno diferida

  La compatibilidad con la expansión de variables de entorno diferida está deshabilitada de forma predeterminada, pero puede habilitarla o deshabilitarla mediante **cmd/v**.
- Trabajar con extensiones de comandos

  Cuando las extensiones de comando están habilitadas (valor predeterminado) y se ejecuta **set** Byly, se muestran todas las variables de entorno actuales. Si ejecuta **set** con un valor, se muestran las variables que coinciden con ese valor.
- Usar **set** en archivos por lotes

  Al crear archivos por lotes, puede usar **set** para crear variables y, a continuación, utilizarlos de la misma manera que usaría las variables numeradas **% 0** hasta **% 9**. También puede usar las variables **% 0** a **% 9** como entrada para **set**.
- Llamar a una variable **set** desde un archivo por lotes

  Cuando llame a un valor de variable desde un archivo por lotes, incluya el valor entre signos de porcentaje ( **%** ). Por ejemplo, si el programa por lotes crea una variable de entorno denominada BAUD, puede usar la cadena asociada a BAUD como parámetro reemplazable escribiendo **% Baud%** en el símbolo del sistema.
- Usar **set** en la consola de recuperación

  El comando **set** , con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="BKMK_examples"></a>Example

Para establecer una variable de entorno denominada TEST ^ 1, escriba:
```
set testVar=test^^1
```

> [!NOTE]
> El comando **set** asigna todo lo que sigue al signo igual (=) al valor de la variable. Si escribe:
> ```
> set testVar="test^1"
> ```
> Obtendrá el siguiente resultado:
> ```
> testVar="test^1"
> ```
> Para establecer una variable de entorno denominada TEST & 1, escriba:
> ```
> set testVar=test^&1
> ```
> Para establecer una variable de entorno denominada INCLUDE, de modo que la cadena C:\Inc (el directorio \Inc de la unidad C) esté asociada a ella, escriba:
> ```
> set include=c:\inc
> ```
> Después, puede usar la cadena C:\Inc en los archivos por lotes, para lo que debe incluir signos de porcentaje ( **%** ). Por ejemplo, puede incluir el siguiente comando en un archivo por lotes para que pueda mostrar el contenido del directorio asociado a la variable de entorno INCLUDE:
> ```
> dir %include%
> ```
> Cuando se procesa este comando, la cadena C:\Inc reemplaza **% include%** .

También puede usar **set** en un programa por lotes que agregue un nuevo directorio a la variable de entorno PATH. Por ejemplo:
```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```
Para mostrar una lista de todas las variables de entorno que comienzan por la letra P, escriba:
```
set p 
```

> [!NOTE]
> Este comando requiere extensiones de comandos, que están habilitadas de forma predeterminada.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)