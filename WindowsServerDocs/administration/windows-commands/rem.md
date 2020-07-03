---
title: rem
description: Artículo de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5161a3ba0904396f29b7c567e3a16da5f95e5271
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933499"
---
# <a name="rem"></a>rem



Registra comentarios (comentarios) en un archivo por lotes o CONFIG.SYS. Si no se especifica ningún comentario, **REM** agrega espaciado vertical.



## <a name="syntax"></a>Sintaxis

```
rem [<Comment>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Comment>|Especifica una cadena de caracteres que se va a incluir como comentario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El comando **REM** no muestra comentarios en la pantalla. Debe usar el comando **echo on** del lote o el archivo de CONFIG.SYS para mostrar comentarios en la pantalla.
-   No se puede usar un carácter de redirección ( **<** o **>** ) o una barra vertical ( **|** ) en un Comentario de archivo por lotes.
-   Aunque puede usar **REM** sin un comentario para agregar espaciado vertical a un archivo por lotes, también puede usar líneas en blanco. Las líneas en blanco se omiten cuando se procesa un programa por lotes.

## <a name="examples"></a>Ejemplos

Para mostrar un archivo por lotes que usa comentarios para los comentarios y para el espaciado vertical:
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause
format b: /v chkdsk b:
```
Para incluir un comentario explicativo antes del comando **prompt** en el archivo de CONFIG.SYS, agregue las líneas siguientes a CONFIG.SYS:
```
rem Set prompt to indicate current directory
prompt $p$g
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)