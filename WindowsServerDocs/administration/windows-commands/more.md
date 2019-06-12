---
title: more
description: 'Tema de los comandos de Windows para ***- '
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
ms.date: 10/16/2017
ms.openlocfilehash: 93ba6c696c509ea20ffe8f680d4416d24d202b89
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437317"
---
# <a name="more"></a>more



Muestra una pantalla de salida a la vez.

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
|           \<Command>           |      Especifica un comando para el que desea mostrar la salida.      |
|               /c               |               Borra la pantalla antes de mostrar una página.               |
|               /p               |                      Expande los caracteres de avance de página.                      |
|               /s               |          Muestra varias líneas en blanco como una sola línea en blanco.          |
|             /t\<N>             |         Muestra las pestañas como el número de espacios especificado por *N*.         |
|             +\<N>              |     Muestra el primer principio de archivo en la línea especificada por *N*.     |
| [\<Drive>:] [<Path>]<FileName> |          Especifica la ubicación y el nombre de un archivo para mostrar.          |
|            \<Archivos >            | Especifica una lista de archivos que se va a mostrar. Separe los nombres de archivo con un espacio. |
|               /?               |                  Muestra la ayuda en el símbolo del sistema.                   |

## <a name="remarks"></a>Comentarios

-   Se aceptan los subcomandos siguientes en el **más** símbolo del sistema (`-- More --`).  
    |Key|Acción|
    |---|------|
    |BARRA ESPACIADORA|Muestra la página siguiente.|
    |ENTRAR|Muestra la línea siguiente.|
    |f|Muestra el siguiente archivo.|
    |q|Se cierra el **más** comando.|
    |=|Muestra el número de línea.|
    |p \<N >|Muestra la siguiente *N* líneas.|
    |s \<N>|Omite la próxima *N* líneas.|
    |?|Muestra los comandos que están disponibles en el **más** símbolo del sistema.|
-Cuando se usa el carácter de redirección ( **<** ), debe especificar un nombre de archivo como origen. Cuando se usa la canalización (**|*), puede usar estos comandos como **dir**, **ordenación**, y **tipo**.
-   El **más** comando, con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="BKMK_examples"></a>Ejemplos

Para ver la primera pantalla de información de un archivo denominado clientes.nue, escriba uno de los siguientes comandos:
```
more < clients.new
type clients.new | more
```
El **más** comando muestra la primera pantalla de información de clientes.nue y, a continuación, muestra el mensaje siguiente:
```
-- More --
```
A continuación, puede presionar la barra espaciadora para ver la siguiente pantalla de información.

Para borrar la pantalla y quitar todas las líneas en blanco adicionales antes de mostrar el archivo clientes.nue, escriba uno de los siguientes comandos:
```
more /c /s < clients.new
type clients.new | more /c /s
```
El **más** comando muestra la primera pantalla de información de clientes.nue y, a continuación, muestra el mensaje siguiente:
```
-- More --
```

### <a name="using-more-subcommands"></a>Usar subcomandos más

Los ejemplos siguientes se pueden usar en el **más** símbolo del sistema (`-- More --`).
- Para mostrar el archivo línea a la vez, presione ENTRAR en el **más** símbolo del sistema.
- Para mostrar la pantalla siguiente, presione la barra espaciadora en el **más** símbolo del sistema.
- Para mostrar el siguiente archivo aparece en la línea de comandos, escriba **f** en el **más** símbolo del sistema.
- ¿Para mostrar los comandos disponibles, escriba **?** en el **más** símbolo del sistema.
- Para salir de **más**, tipo **q** en el **más** símbolo del sistema.
- Para mostrar el número de línea actual, escriba **=** en el **más** símbolo del sistema. El número de línea actual se agrega a la **más** símbolo del sistema como sigue:  
  ```
  -- More [Line: 24] --
  ```  
- Para mostrar un número específico de líneas, escriba **p** en el **más** símbolo del sistema. **Más** le solicita que el número de líneas que desea mostrar como sigue:  
  ```
  -- More -- Lines:
  ```  
  Escriba el número de líneas para mostrar y, a continuación, presione ENTRAR. **Más** muestra el número de líneas especificado.
- Para omitir un número específico de líneas, escriba **s** en el **más** símbolo del sistema. **Más** le solicita que el número de líneas para omitir la manera siguiente:  
  ```
  -- More -- Lines:
  ```  
  Escriba el número de líneas para omitir y, a continuación, presione ENTRAR. **Más** omite el número especificado de líneas y muestra la siguiente pantalla de información.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)