---
title: more
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: 291d98492f3f2b200ff0567c28a97927ca8c75be
ms.sourcegitcommit: e55e27143dad1d3fb956cfdac4c23ef4186af321
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603232"
---
# <a name="more"></a>more



Muestra una pantalla de salida cada vez.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

## <a name="parameters"></a>Parámetros

|           Parámetro            |                               Descripción                               |
|--------------------------------|-------------------------------------------------------------------------|
|           \<> De comandos           |      Especifica un comando para el que desea mostrar la salida.      |
|               /c               |               Borra la pantalla antes de mostrar una página.               |
|               /p               |                      Expande los caracteres de avance de la forma.                      |
|               /s               |          Muestra varias líneas en blanco como una sola línea en blanco.          |
|             /t\<N >             |         Muestra las pestañas como el número de espacios especificado por *N*.         |
|             +\<N >              |     Muestra el primer archivo que comienza en la línea especificada por *N*.     |
| [\<Unidad >:] [\<ruta de acceso\<>] nombre de archivo > |          Especifica la ubicación y el nombre de un archivo que se va a mostrar.          |
|            \<Archivos >            | Especifica una lista de archivos que se van a mostrar. Separe los nombres de archivo con un espacio. |
|               /?               |                  Muestra la ayuda en el símbolo del sistema.                   |

## <a name="remarks"></a>Comentarios

-   Los siguientes subcomandos se aceptan en el  símbolo del sistema`-- More --`más (). 

    | Key | . |
    | --- | ------ |
    | TECLA | Muestra la página siguiente. |
    | ENTRAR | Muestra la línea siguiente. |
    | f | Muestra el siguiente archivo. |
    | q | Sale del comando **More** . |
    | = | Muestra el número de línea. |
    | p \<N > | Muestra las siguientes *N* líneas. |
    | s \<N > |S Kips las siguientes *N* líneas. |
    | ? | Muestra los comandos que están disponibles en el símbolo del sistema **más** .| 
    
-   Al usar el carácter de redirección **<** (), debe especificar un nombre de archivo como origen. Cuando se usa **\|** la canalización (), puede usar estos comandos como **dir**, **Sort**y **Type**.
-   El comando **More** , con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="BKMK_examples"></a>Example

Para ver la primera pantalla de información de un archivo denominado clients. New, escriba uno de los siguientes comandos:
```
more < clients.new
type clients.new | more
```
El comando **More** muestra la primera pantalla de información de clients. New y, a continuación, muestra el siguiente mensaje:
```
-- More --
```
A continuación, puede presionar la barra ESPACIAdora para ver la siguiente pantalla de información.

Para borrar la pantalla y quitar todas las líneas en blanco adicionales antes de mostrar el archivo clients. New, escriba uno de los siguientes comandos:
```
more /c /s < clients.new
type clients.new | more /c /s
```
El comando **More** muestra la primera pantalla de información de clients. New y, a continuación, muestra el siguiente mensaje:
```
-- More --
```

### <a name="using-more-subcommands"></a>Usar más subcomandos

Los ejemplos siguientes se pueden usar en el  símbolo del sistema`-- More --`más ().
- Para mostrar el archivo una línea a la vez, presione Entrar en el símbolo del sistema **más** .
- Para mostrar la siguiente pantalla, presione la barra ESPACIAdora en el símbolo del sistema **más** .
- Para mostrar el siguiente archivo que aparece en la línea de comandos, escriba **f** en el símbolo del sistema **más** .
- Para mostrar los comandos disponibles, escriba **?** en el símbolo del sistema **más** .
- Para salir **más**, escriba **q** en el símbolo del sistema **más** .
- Para mostrar el número de línea actual, **=** escriba en el símbolo del sistema **más** . El número de línea actual se agrega al símbolo **más** como se indica a continuación:  
  ```
  -- More [Line: 24] --
  ```  
- Para mostrar un número específico de líneas, escriba **p** en el símbolo del sistema **más** . **Más** solicita el número de líneas que se mostrarán de la siguiente manera:  
  ```
  -- More -- Lines:
  ```  
  Escriba el número de líneas que desea mostrar y, a continuación, presione Entrar. **Más** muestra el número de líneas especificado.
- Para omitir un número específico de líneas, escriba **s** en el símbolo del sistema **más** . **Más** solicita el número de líneas que se omitirán de la siguiente manera:  
  ```
  -- More -- Lines:
  ```  
  Escriba el número de líneas que desea omitir y, a continuación, presione Entrar. **More** omite el número de líneas especificado y muestra la siguiente pantalla de información.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
