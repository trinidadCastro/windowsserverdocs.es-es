---
title: endlocal
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 16d2b7b445a2220a10f88f21029948ed10ee96e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377574"
---
# <a name="endlocal"></a>endlocal



Finaliza la localización de los cambios de entorno en un archivo por lotes y restaura las variables de entorno a sus valores antes de ejecutar el comando **setlocal** correspondiente.

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

-   El comando **endlocal** no tiene ningún efecto fuera de un script o un archivo por lotes.
-   Hay un comando de **endlocal** implícito al final de un archivo por lotes.
-   Si las extensiones de comandos están habilitadas (las extensiones de comandos están habilitadas de forma predeterminada), el comando **endlocal** restaura el estado de las extensiones de comando (es decir, habilitado o deshabilitado) a lo que estaba antes de que se ejecutara el comando **setlocal** correspondiente.

> [!NOTE]
> Para obtener más información acerca de cómo habilitar y deshabilitar las extensiones de comandos, vea [cmd](cmd.md).

## <a name="BKMK_examples"></a>Example

Puede localizar las variables de entorno en un archivo por lotes. Por ejemplo, el siguiente programa inicia el programa por lotes superapp en la red, dirige la salida a un archivo y muestra el archivo en el Bloc de notas:
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