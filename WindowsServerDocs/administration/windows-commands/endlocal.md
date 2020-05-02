---
title: endlocal
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f007a9ec1e86093192630011c5197740dfefe922
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719375"
---
# <a name="endlocal"></a>endlocal



Finaliza la localización de los cambios de entorno en un archivo por lotes y restaura las variables de entorno a sus valores antes de ejecutar el comando **setlocal** correspondiente.



## <a name="syntax"></a>Sintaxis

```
endlocal
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   El comando **endlocal** no tiene ningún efecto fuera de un script o un archivo por lotes.
-   Hay un comando de **endlocal** implícito al final de un archivo por lotes.
-   Si las extensiones de comandos están habilitadas (las extensiones de comandos están habilitadas de forma predeterminada), el comando **endlocal** restaura el estado de las extensiones de comando (es decir, habilitado o deshabilitado) a lo que estaba antes de que se ejecutara el comando **setlocal** correspondiente.

> [!NOTE]
> Para obtener más información acerca de cómo habilitar y deshabilitar las extensiones de comandos, vea [cmd](cmd.md).

## <a name="examples"></a>Ejemplos

Puede localizar las variables de entorno en un archivo por lotes. Por ejemplo, el siguiente programa inicia el programa por lotes superapp en la red, dirige la salida a un archivo y muestra el archivo en el Bloc de notas:
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)