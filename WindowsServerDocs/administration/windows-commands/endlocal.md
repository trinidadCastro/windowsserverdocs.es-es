---
title: endlocal
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4958c5419ed4f6374f7c6ecf09bdf67f61134d93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845118"
---
# <a name="endlocal"></a>endlocal



Finaliza la localización de los cambios de entorno en un archivo por lotes y restaura las variables de entorno a sus valores antes de ejecutar el comando **setlocal** correspondiente.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
endlocal
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El comando **endlocal** no tiene ningún efecto fuera de un script o un archivo por lotes.
-   Hay un comando de **endlocal** implícito al final de un archivo por lotes.
-   Si las extensiones de comandos están habilitadas (las extensiones de comandos están habilitadas de forma predeterminada), el comando **endlocal** restaura el estado de las extensiones de comando (es decir, habilitado o deshabilitado) a lo que estaba antes de que se ejecutara el comando **setlocal** correspondiente.

> [!NOTE]
> Para obtener más información acerca de cómo habilitar y deshabilitar las extensiones de comandos, vea [cmd](cmd.md).

## <a name="examples"></a><a name=BKMK_examples></a>Example

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