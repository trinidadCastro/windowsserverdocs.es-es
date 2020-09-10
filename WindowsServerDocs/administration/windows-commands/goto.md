---
title: goto
description: Artículo de referencia para el comando Goto, que dirige cmd.exe a una línea etiquetada en un programa por lotes.
ms.topic: reference
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 537026fc2b4faafa57b7a4f2842d79775759cdc9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634804"
---
# <a name="goto"></a>goto

Dirige cmd.exe a una línea etiquetada en un programa por lotes. Dentro de un programa por lotes, este comando dirige el procesamiento de comandos a una línea identificada por una etiqueta. Cuando se encuentra la etiqueta, el procesamiento continúa a partir de los comandos que comienzan en la línea siguiente.

## <a name="syntax"></a>Sintaxis

```
goto <label>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<label>` | Especifica una cadena de texto que se utiliza como etiqueta en el programa por lotes. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

-  Si las extensiones de comando están habilitadas (el valor predeterminado) y se usa el comando **goto** con una etiqueta de destino de **: EOF**, se transfiere el control al final del archivo de script por lotes actual y se sale del archivo de script por lotes sin definir una etiqueta. Cuando use este comando con la etiqueta **: EOF** , debe insertar un signo de dos puntos delante de la etiqueta. Por ejemplo: `goto:EOF`.

- Puede usar espacios en el parámetro de *etiqueta* , pero no puede incluir otros separadores (por ejemplo, puntos y comas (;) o signo igual (=)).

- El valor de *etiqueta* que especifique debe coincidir con una etiqueta en el programa por lotes. La etiqueta del programa por lotes debe comenzar con dos puntos (:). Si una línea comienza con un signo de dos puntos, se trata como una etiqueta y se omiten los comandos de esa línea. Si el programa por lotes no contiene la etiqueta que se especifica en el parámetro *etiqueta* , el programa por lotes se detiene y muestra el mensaje siguiente: `Label not found` .

- Puede usar **goto** con otros comandos para realizar operaciones condicionales. Para obtener más información sobre el uso de **goto** para las operaciones condicionales, vea el [comando if](if.md).

## <a name="examples"></a>Ejemplos

El siguiente programa por lotes da formato a un disco de la unidad A como un disco del sistema. Si la operación se realiza correctamente, el comando **goto** dirige el procesamiento a la etiqueta **: end** :

```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [CMD (comando)](cmd.md)

- [if (comando)](if.md)