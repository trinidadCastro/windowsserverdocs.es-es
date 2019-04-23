---
title: endlocal
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e516b2bf9e8a45ada910dfbd93e3ed5e7d86c14
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862146"
---
# <a name="endlocal"></a>endlocal



Finaliza la localización de los cambios de entorno en un archivo por lotes y se restauran las variables de entorno en sus valores antes de la correspondiente **setlocal** se ejecutó el comando.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
endlocal
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **endlocal** comando no tiene ningún efecto fuera de un secuencia de comandos o archivo por lotes.
-   Hay un modo implícito **endlocal** comando al final de un archivo por lotes.
-   Si se habilitan las extensiones de comando (extensiones de comando están habilitadas de forma predeterminada), el **endlocal** comando restaura el estado de las extensiones de comando (es decir, habilitado o deshabilitado) que tenía antes de la correspondiente  **Setlocal** se ejecutó el comando.

> [!NOTE]
> Para obtener más información sobre cómo habilitar y deshabilitar las extensiones de comando, consulte [Cmd](cmd.md).

## <a name="BKMK_examples"></a>Ejemplos

Puede localizar las variables de entorno en un archivo por lotes. Por ejemplo, el programa siguiente inicia el programa de versión por lotes en la red, dirige la salida a un archivo y muestra el archivo en el Bloc de notas:
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)