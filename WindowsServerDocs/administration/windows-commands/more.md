---
title: más
description: Tema de referencia del comando More, que muestra una pantalla de salida cada vez.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: 042669aa638990375157d08d9e12840ade486165
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354575"
---
# <a name="more"></a>más

Muestra una pantalla de salida cada vez.

> [!NOTE]
> El comando **More** , con parámetros diferentes, también está disponible en la consola de recuperación.

## <a name="syntax"></a>Sintaxis

```
<command> | more [/c] [/p] [/s] [/t<n>] [+<n>]
more [[/c] [/p] [/s] [/t<n>] [+<n>]] < [<drive>:][<path>]<filename>
more [/c] [/p] [/s] [/t<n>] [+<n>] [<files>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<command>` | Especifica un comando para el que desea mostrar la salida. |
| /C | Borra la pantalla antes de mostrar una página. |
| /p | Expande los caracteres de avance de la forma. |
| /s | Muestra varias líneas en blanco como una sola línea en blanco. |
| /t`<n>` | Muestra las pestañas como el número de espacios especificado por *n*. |
| +`<n>` | Muestra el primer archivo, comenzando en la línea especificada por *n*. |
| `[<drive>:][<path>]<filename>` | Especifica la ubicación y el nombre de un archivo que se va a mostrar. |
| `<files>` | Especifica una lista de archivos que se van a mostrar. Los archivos se deben separar mediante espacios. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los siguientes subcomandos se aceptan en el símbolo del sistema **más** ( `-- More --` ), lo que incluye:

    | Clave | Acción |
    | --- | ------ |
    | BARRA ESPACIADORA | Presione la **barra espaciadora** para mostrar la pantalla siguiente. |
    | ENTRAR | Presione **entrar** para mostrar el archivo una línea a la vez. |
    | f | Presione **F** para mostrar el siguiente archivo que aparece en la línea de comandos. |
    | q | Presione **Q** para salir del comando **más** . |
    | = | Muestra el número de línea. |
    | m`<n>` | Presione **P** para mostrar las siguientes *n* líneas. |
    | seg`<n>` | Presione **S** para omitir las siguientes *n* líneas. |
    | ? | **¿Presiona?** para mostrar los comandos que están disponibles en el símbolo del sistema **más** .|

- Si usa el carácter de redirección ( `<` ), también debe especificar un nombre de archivo como origen.

- Si usa la canalización ( `|` ), puede usar estos comandos como **dir**, **Sort**y **Type**.

### <a name="examples"></a>Ejemplos

Para ver la primera pantalla de información de un archivo denominado *clients. New*, escriba uno de los siguientes comandos:

```
more < clients.new
type clients.new | more
```

El comando **More** muestra la primera pantalla de información de clients. New y puede presionar la barra espaciadora para ver la siguiente pantalla de información.

Para borrar la pantalla y quitar todas las líneas en blanco adicionales antes de mostrar el archivo *clients. New*, escriba uno de los siguientes comandos:

```
more /c /s < clients.new
type clients.new | more /c /s
```

Para mostrar el número de línea actual en el símbolo del sistema **más** , escriba:

```
more =
```

El número de línea actual se agrega al símbolo **más** , como`-- More [Line: 24] --`

Para mostrar un número específico de líneas en el símbolo **más** , escriba:

```
more p
```

El mensaje **más** le pide el número de líneas que se van a mostrar, como se indica a continuación: `-- More -- Lines:` . Escriba el número de líneas que desea mostrar y, a continuación, presione Entrar. La pantalla cambia para mostrar solo ese número de líneas.

Para omitir un número específico de líneas en el símbolo **más** , escriba:

```
more s
```

El mensaje **más** solicita el número de líneas que se van a omitir, como se indica a continuación: `-- More -- Lines:` . Escriba el número de líneas que desea omitir y, a continuación, presione Entrar. La pantalla cambia para mostrar que las líneas se omiten.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Entorno de recuperación de Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
