---
title: set
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ece1581bad3d78add93a5bac2c4331ebc5240eef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882136"
---
# <a name="set"></a>set



Muestra, Establece o quita CMD. Variables de entorno del archivo EXE. Si se utiliza sin parámetros, **establecer** muestra la configuración actual de la variable de entorno.

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
|\<Variable>|Especifica la variable de entorno para establecer o modificar.|
|\<String>|Especifica la cadena para asociar a la variable de entorno especificada.|
|/p|Establece el valor de *Variable* a una línea de entrada especificado por el usuario.|
|\<PromptString>|Opcional. Especifica un mensaje para pedir al usuario para la entrada. Este parámetro se usa con el **/p** opción de línea de comandos.|
|/a|Conjuntos de *cadena* en una expresión numérica que se evalúa.|
|\<expresión >|Especifica una expresión numérica. Vea la sección Comentarios para los operadores válidos que pueden usarse en *expresión*.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Uso de **establecer** están habilitadas las extensiones de comando

    Cuando se habilitan las extensiones de comando (el valor predeterminado) y ejecutar **establecer** con un valor, muestra todas las variables que comienzan con ese valor.
-   Uso de caracteres especiales

    Los caracteres **<**, **>**, **|**, **&**, **^** son caracteres de shell de comandos especial, y deben ir precedidos por el carácter de escape (**^**) o entre comillas cuando se usa en *cadena* (por ejemplo, **"CadenaContiene & símbolo"**). Si utiliza comillas para delimitar una cadena que contiene uno de los caracteres especiales, las comillas se establecen como parte del valor de variable de entorno.
-   Usar variables de entorno

    Usar variables de entorno para controlar el comportamiento de algunos programas y archivos por lotes y para controlar la manera Windows y el subsistema de MS-DOS aparece y funciona. El **establecer** comando a menudo se usa en el archivo Autoexec.nt para establecer las variables de entorno.
-   Mostrar la configuración del entorno actual

    Cuando se escribe el **establecer** comando por sí solo, se muestra la configuración del entorno actual. Normalmente, estas opciones incluyen las variables de entorno COMSPEC y ruta de acceso, que se usan para buscar programas en el disco. Windows usa las variables de entorno son el símbolo del sistema y DIRCMD.
-   Uso de parámetros

    Al especificar valores para *Variable* y *cadena*, especificado *variable* valor se agrega al entorno y *cadena* es asociado a esa variable. Si la variable ya existe en el entorno, el nuevo valor de cadena reemplaza el valor de cadena antiguos.

    Si especifica solamente una variable y un signo igual (sin *cadena*) para el **establecer** comando, el *cadena* se borra el valor asociado a la variable (como si la variable no existe).
-   Uso de **/a**

    En la tabla siguiente se enumera los operadores admitidos para **/a** en orden descendente de prioridad.  
    |Operador|Operación realizada|
    |--------|-------------------|
    |( )|Agrupar|
    |! ~ -|Unario|
    |* / %|operaciones aritméticas|
    |+ -|operaciones aritméticas|
    |<< >>|Desplazamiento lógico|
    |&|AND bit a bit|
    |^|Bit a bit OR exclusivo|
    |||OR bit a bit|
    |= *= /= %= += -= &= ^= |= <<= >>=|Asignación|
    |,|Separador de expresiones|

    Si usas lógico (**&&** o **||**) o de módulo (**%**) de los operadores, escriba la cadena de expresión entre comillas. Cualquier cadena no numérico de la expresión se considera nombres de variable de entorno y sus valores se convierten a números antes de ser procesados. Si especifica un nombre de variable de entorno que no está definido en el entorno actual, se asigna un valor de cero, lo que permite realizar operaciones aritméticas con valores de variables de entorno sin utilizar % para recuperar un valor.

    Si ejecuta **set /a** desde la línea de comandos fuera de una secuencia de comandos, muestra el valor final de la expresión.

    Los valores numéricos son números decimales comprendidos a menos que el prefijo 0 x para números hexadecimales o 0 para números octales. Por lo tanto, 0 x 12 es igual a 18, que es el mismo que 022.
-   Compatibilidad con la expansión de variables de entorno retrasada

    Compatibilidad con la expansión de variables de entorno retrasada está deshabilitada de forma predeterminada, pero puede habilitar o deshabilitar utilizando **cmd /v**.
-   Trabajar con las extensiones de comando

    Cuando se habilitan las extensiones de comando (el valor predeterminado) y ejecutar **establecer** por sí solo, muestra todas las variables de entorno actual. Si ejecuta **establecer** con un valor, se muestran las variables que coinciden con ese valor.
-   Uso de **establecer** en archivos por lotes

    Al crear archivos por lotes, puede usar **establecer** crear variables y, a continuación, utilizarlas en la misma manera que utilizaría las variables numeradas **%0** a través de **%9**. También puede usar las variables **%0** a través de **%9** como entrada para **establecer**.
-   Una llamada a un **establecer** variable desde un archivo por lotes

    Cuando se llama a un valor de la variable desde un archivo por lotes, incluya el valor con signos de porcentaje (**%**). Por ejemplo, si el programa por lotes crea una variable de entorno denominada BAUD, puede usar la cadena asociada con la velocidad en baudios como un parámetro reemplazable escribiendo **baudios %** en el símbolo del sistema.
-   Uso de **establecer** en la consola de recuperación

    El **establecer** comando, con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="BKMK_examples"></a>Ejemplos

Para establecer una variable de entorno llamada TEST ^ 1, escriba:
```
set testVar=test^^1
```

> [!NOTE]
> El **establecer** comando asigna todo lo que sigue el signo igual (=) en el valor de la variable. Si escribe:
```
set testVar="test^1"
```
Obtiene el siguiente resultado:
```
testVar="test^1"
```
Para establecer una variable de entorno llamada TEST & 1, escriba:
```
set testVar=test^&1
```
Para establecer una variable de entorno denominada INCLUDE para que la cadena C:\Inc (el directorio \Inc en la unidad C) está asociada a ella, escriba:
```
set include=c:\inc
```
A continuación, puede usar la cadena C:\Inc en archivos por lotes, incluya el nombre de la inclusión con signos de porcentaje (**%**). Por ejemplo, es posible que incluya el siguiente comando en un archivo por lotes para que pueda mostrar el contenido del directorio que está asociado a la variable de entorno INCLUDE:
```
dir %include%
```
Cuando se procesa este comando, la cadena C:\Inc reemplaza **% include %**.

También puede usar **establecer** en un programa por lotes que se agrega un nuevo directorio a la variable de entorno PATH. Por ejemplo:
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
> Este comando requiere las extensiones de comando, que están habilitadas de forma predeterminada.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)