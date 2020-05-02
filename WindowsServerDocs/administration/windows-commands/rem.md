---
title: rem
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d115548f15ff45087a771458062da8a3ef919eb3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722461"
---
# <a name="rem"></a>rem



Registra comentarios (comentarios) en un archivo por lotes o una configuración. Sist. Si no se especifica ningún comentario, **REM** agrega espaciado vertical.



## <a name="syntax"></a>Sintaxis

```
rem [<Comment>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Comentario>|Especifica una cadena de caracteres que se va a incluir como comentario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   El comando **REM** no muestra comentarios en la pantalla. Debe usar el comando **echo on** en el lote o la configuración. Archivo SYS para mostrar los comentarios en la pantalla.
-   No se puede usar un carácter de**<** redirección **>**(o) o**|** una barra vertical () en un Comentario de archivo por lotes.
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
Para incluir un comentario explicativo antes del comando **prompt** en la configuración. SYS, agregue las líneas siguientes al archivo CONFIG. Sist
```
rem Set prompt to indicate current directory
prompt $p$g
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)