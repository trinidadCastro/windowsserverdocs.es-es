---
title: goto
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f1ad0190519d58bd879ae391f378d800760c204f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857536"
---
# <a name="goto"></a>goto



Dirige cmd.exe a una línea de un programa por lotes. Dentro de un programa por lotes, **goto** dirige el procesamiento de comandos en una línea que se identifica mediante una etiqueta. Cuando se encuentra la etiqueta, el procesamiento continúa a partir de los comandos que comienzan en la línea siguiente.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
goto <Label> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Etiqueta >|Especifica una cadena de texto que se usa como una etiqueta en el programa por lotes.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Trabajar con las extensiones de comando

    Si las extensiones de comando son habilitado (valor predeterminado), y usar el **goto** comando con una etiqueta de destino **: EOF**, transferir el control al final del archivo de script por lotes actual y salir del archivo de script por lotes sin definir una etiqueta. Cuando usas **goto** con el **: EOF** etiqueta, debe insertar un signo de dos puntos antes de la etiqueta. Por ejemplo:  
    ```
    goto:EOF
    ```  
-   Uso válido *etiqueta* valores

    Puede usar espacios en el *etiqueta* parámetro, pero no puede incluir otros separadores (por ejemplo, punto y coma o signos igual).
-   Coincidencia *etiqueta* con la etiqueta en el programa por lotes

    El *etiqueta* valor que especifique debe coincidir con una etiqueta en el programa por lotes. La etiqueta dentro del programa por lotes debe comenzar con un signo de dos puntos (:). Si una línea comienza con un signo de dos puntos, se trata como una etiqueta y se omiten los comandos en esa línea. Si el programa por lotes no contiene la etiqueta que especifique en *etiqueta*, el programa por lotes se detiene y se muestra el mensaje siguiente:  
    ```
    Label not found
    ```  
-   Uso de **goto** para operaciones condicionales

    Puede usar **goto** con otros comandos para realizar operaciones condicionales. Para obtener más información sobre el uso de **goto** para operaciones condicionales, consulte el [si](if.md) referencia del comando.

## <a name="BKMK_examples"></a>Ejemplos

El siguiente programa por lotes da formato a un disco de la unidad como un disco del sistema. Si la operación se realiza correctamente, el **goto** comando dirige el procesamiento a la **: final** etiqueta:
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

[If](if.md)