---
title: rem
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2da0b6e42858582c1485659f3bf8f59e8e2ed97e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384568"
---
# <a name="rem"></a>rem



Registra comentarios (comentarios) en un archivo por lotes o una configuración. Sist. Si no se especifica ningún comentario, **REM** agrega espaciado vertical.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
rem [<Comment>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Comment >|Especifica una cadena de caracteres que se va a incluir como comentario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El comando **REM** no muestra comentarios en la pantalla. Debe usar el comando **echo on** en el lote o la configuración. Archivo SYS para mostrar los comentarios en la pantalla.
-   No se puede usar un carácter de redirección ( **<** o **>** ) o una canalización ( **|** ) en un Comentario de archivo por lotes.
-   Aunque puede usar **REM** sin un comentario para agregar espaciado vertical a un archivo por lotes, también puede usar líneas en blanco. Las líneas en blanco se omiten cuando se procesa un programa por lotes.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se muestra un archivo por lotes que usa comentarios para los comentarios y para el espaciado vertical:
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

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)