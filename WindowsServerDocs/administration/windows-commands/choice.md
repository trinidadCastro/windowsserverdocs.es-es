---
title: choice
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78d2816bff754ef04558cf37eaada7c7fafba823
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841556"
---
# <a name="choice"></a>choice



Pide al usuario seleccionar un elemento de una lista de opciones de carácter único en un programa por lotes y, a continuación, devuelve el índice de la opción seleccionada. Si se utiliza sin parámetros, **elección** muestra las opciones predeterminadas **Y** y **N**.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <"Text">]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/c \<Choice1 ><Choice2><... >|Especifica la lista de opciones que se va a crear. Las opciones válidas incluyen, a-z, A-z, 0-9 y caracteres ASCII extendidos (128-254). La lista predeterminada es "YN", que se muestra como `[Y,N]?`.|
|/n|Oculta la lista de opciones, aunque todavía están habilitadas las opciones y el texto del mensaje (si se especifica por **/m**) se sigue mostrando.|
|/cs|Especifica que las opciones distinguen mayúsculas de minúsculas. De forma predeterminada, las opciones no distinguen mayúsculas de minúsculas.|
|/t \<tiempo de espera >|Especifica el número de segundos que transcurrirán antes de usar la opción predeterminada especificada por **/d**. Los valores aceptables son de **0** a **9999**. Si **/t** está establecido en **0**, **elección** no realiza una pausa antes de devolver la opción predeterminada.|
|/d \<choice >|Especifica la opción predeterminada para usar después de esperar el número de segundos especificado por **/t**. La opción predeterminada debe estar en la lista de opciones especificada por **/c**.|
|/m <"Text">|Especifica un mensaje para mostrar antes de la lista de opciones. Si **/m** no se especifica, se muestra solo la opción símbolo del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   La variable de entorno ERRORLEVEL se establece en el índice de la clave que el usuario selecciona en la lista de opciones. La primera opción en la lista, devuelve un valor de 1, la segunda un valor de 2 y así sucesivamente. Si el usuario presiona una tecla que no es una opción válida, **elección** suena un sonido de advertencia. Si **elección** detecta una condición de error, devuelve un valor ERRORLEVEL de 255. Si el usuario presione CTRL+PAUSA o CTRL+C **elección** devuelve un valor ERRORLEVEL de 0.

> [!NOTE]
> Cuando se utilizan valores ERRORLEVEL en un programa por lotes, debe enumerarlos en orden decreciente.

## <a name="BKMK_examples"></a>Ejemplos

Para presentar las opciones Y, N y C, escriba la siguiente línea en un archivo por lotes:
```
choice /c ync
```
El siguiente mensaje aparece cuando se ejecuta el archivo por lotes la **elección** comando:
```
[Y,N,C]?
```
Para ocultar las opciones Y, N y C, pero muestra el texto "Sí, No o continuar", escriba la siguiente línea en un archivo por lotes:
```
choice /c ync /n /m "Yes, No, or Continue?"
```
El siguiente mensaje aparece cuando se ejecuta el archivo por lotes la **elección** comando:
```
Yes, No, or Continue?
```

> [!NOTE]
> Si usas el **/n** parámetro, pero no use **/m**, el usuario no es notificado cuando **elección** está esperando una entrada.

Para mostrar el texto y las opciones utilizadas en los ejemplos anteriores, escriba la siguiente línea en un archivo por lotes:
```
choice /c ync /m "Yes, No, or Continue"
```
El siguiente mensaje aparece cuando se ejecuta el archivo por lotes la **elección** comando:
```
Yes, No, or Continue [Y,N,C]?
```
Para establecer un límite de tiempo de cinco segundos y especificar **N** como el valor predeterminado, escriba la siguiente línea en un archivo por lotes:
```
choice /c ync /t 5 /d n
```
El siguiente mensaje aparece cuando se ejecuta el archivo por lotes la **elección** comando:
```
[Y,N,C]?
```

> [!NOTE]
> En este ejemplo, si el usuario no presione una tecla dentro de cinco segundos, **elección** selecciona **N** de forma predeterminada y devuelve un valor de error de 2. En caso contrario, **elección** devuelve el valor correspondiente a la elección del usuario.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)