---
title: rem
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94428e6d5ec6fdb482a5d0d15bd1120e45ffea80
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836118"
---
# <a name="rem"></a>rem



Registra comentarios (comentarios) en un archivo por lotes o una configuración. Sist. Si no se especifica ningún comentario, **REM** agrega espaciado vertical.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
rem [<Comment>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<comentario >|Especifica una cadena de caracteres que se va a incluir como comentario.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El comando **REM** no muestra comentarios en la pantalla. Debe usar el comando **echo on** en el lote o la configuración. Archivo SYS para mostrar los comentarios en la pantalla.
-   No se puede usar un carácter de redirección ( **<** o **>** ) o una canalización ( **|** ) en un Comentario de archivo por lotes.
-   Aunque puede usar **REM** sin un comentario para agregar espaciado vertical a un archivo por lotes, también puede usar líneas en blanco. Las líneas en blanco se omiten cuando se procesa un programa por lotes.

## <a name="examples"></a><a name=BKMK_examples></a>Example

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

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)