---
title: goto
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1caf3da3e8b873150af5be7ed8316cfcb526db83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375693"
---
# <a name="goto"></a>goto



Dirige cmd. exe a una línea etiquetada en un programa por lotes. En un programa por lotes, **goto** dirige el procesamiento de comandos a una línea identificada por una etiqueta. Cuando se encuentra la etiqueta, el procesamiento continúa a partir de los comandos que comienzan en la línea siguiente.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
goto <Label> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<etiqueta >|Especifica una cadena de texto que se utiliza como etiqueta en el programa por lotes.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Trabajar con extensiones de comandos

    Si las extensiones de comando están habilitadas (el valor predeterminado) y se usa el comando **goto** con una etiqueta de destino de **: EOF**, se transfiere el control al final del archivo de script por lotes actual y se sale del archivo de script por lotes sin definir una etiqueta. Cuando use **goto** con la etiqueta **: EOF** , debe insertar un signo de dos puntos delante de la etiqueta. Por ejemplo:  
    ```
    goto:EOF
    ```  
-   Usar valores de *etiqueta* válidos

    Puede usar espacios en el parámetro de *etiqueta* , pero no puede incluir otros separadores (por ejemplo, signos de punto y coma o signo igual).
-   *Etiqueta* coincidente con la etiqueta en el programa por lotes

    El valor de *etiqueta* que especifique debe coincidir con una etiqueta en el programa por lotes. La etiqueta del programa por lotes debe comenzar con dos puntos (:). Si una línea comienza con un signo de dos puntos, se trata como una etiqueta y se omiten los comandos de esa línea. Si el programa por lotes no contiene la etiqueta que se especifica en la *etiqueta*, el programa por lotes se detiene y muestra el mensaje siguiente:  
    ```
    Label not found
    ```  
-   Usar **goto** para operaciones condicionales

    Puede usar **goto** con otros comandos para realizar operaciones condicionales. Para obtener más información sobre el uso de **goto** para las operaciones condicionales, vea la referencia del comando [If](if.md) .

## <a name="BKMK_examples"></a>Example

El siguiente programa por lotes da formato a un disco de la unidad A como un disco del sistema. Si la operación se realiza correctamente, el comando **goto** dirige el procesamiento a la etiqueta **: end** :
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Cmd](cmd.md)

[Cuando](if.md)