---
title: choice
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e710b3813525647053365ebf4c764181fc38b3f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379372"
---
# <a name="choice"></a>choice



Solicita al usuario que seleccione un elemento de una lista de opciones de un solo carácter en un programa por lotes y, a continuación, devuelve el índice de la opción seleccionada. Si se usa sin parámetros, **Choice** muestra **las opciones predeterminadas y y** **N**.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <"Text">]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/c \<Choice1 > <Choice2> <... >|Especifica la lista de opciones que se van a crear. Entre las opciones válidas se incluyen los caracteres a-z, A-Z, 0-9 y ASCII extendido (128-254). La lista predeterminada es "YN", que se muestra como `[Y,N]?`.|
|/n|Oculta la lista de opciones, aunque las opciones siguen estando habilitadas y se sigue mostrando el texto del mensaje (si se especifica en **/m**).|
|/CS|Especifica que las opciones distinguen mayúsculas de minúsculas. De forma predeterminada, las opciones no distinguen mayúsculas de minúsculas.|
|/t \<Timeout >|Especifica el número de segundos que se van a pausar antes de usar la opción predeterminada especificada por **/d**. Los valores aceptables son de **0** a **9999**. Si **/t** se establece en **0**, **Choice** no se pausa antes de devolver la opción predeterminada.|
|/d \<Choice >|Especifica la opción predeterminada que se debe usar después de esperar el número de segundos especificado por **/t**. La opción predeterminada debe estar en la lista de opciones especificada por **/c**.|
|/m < "Text" >|Especifica el mensaje que se va a mostrar antes de la lista de opciones. Si no se especifica **/m** , solo se muestra el mensaje de elección.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   La variable de entorno ERRORLEVEL se establece en el índice de la clave que el usuario selecciona en la lista de opciones. La primera opción de la lista devuelve un valor de 1, el segundo un valor de 2, y así sucesivamente. Si el usuario presiona una tecla que no es una opción válida, la **opción** suena un pitido de advertencia. Si **Choice** detecta una condición de error, devuelve un valor ERRORLEVEL de 255. Si el usuario presiona CTRL + INTER o CTRL + C, **Choice** devuelve un valor ERRORLEVEL de 0.

> [!NOTE]
> Cuando se usan valores ERRORLEVEL en un programa por lotes, se muestran en orden decreciente.

## <a name="BKMK_examples"></a>Example

Para presentar las opciones Y, N y C, escriba la siguiente línea en un archivo por lotes:
```
choice /c ync
```
El siguiente mensaje aparece cuando el archivo por lotes ejecuta el comando **Choice** :
```
[Y,N,C]?
```
Para ocultar las opciones Y, N y C, pero mostrar el texto "sí, no o continuar", escriba la siguiente línea en un archivo por lotes:
```
choice /c ync /n /m "Yes, No, or Continue?"
```
El siguiente mensaje aparece cuando el archivo por lotes ejecuta el comando **Choice** :
```
Yes, No, or Continue?
```

> [!NOTE]
> Si usa el parámetro **/n** , pero no usa **/m**, no se le pedirá al usuario cuando la **opción** está esperando una entrada.

Para mostrar tanto el texto como las opciones que se usan en los ejemplos anteriores, escriba la siguiente línea en un archivo por lotes:
```
choice /c ync /m "Yes, No, or Continue"
```
El siguiente mensaje aparece cuando el archivo por lotes ejecuta el comando **Choice** :
```
Yes, No, or Continue [Y,N,C]?
```
Para establecer un límite de tiempo de cinco segundos y especificar **N** como valor predeterminado, escriba la siguiente línea en un archivo por lotes:
```
choice /c ync /t 5 /d n
```
El siguiente mensaje aparece cuando el archivo por lotes ejecuta el comando **Choice** :
```
[Y,N,C]?
```

> [!NOTE]
> En este ejemplo, si el usuario no presiona una tecla en cinco segundos, **Choice** selecciona **N** de forma predeterminada y devuelve un valor de error de 2. De lo contrario, **Choice** devuelve el valor correspondiente a la elección del usuario.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)