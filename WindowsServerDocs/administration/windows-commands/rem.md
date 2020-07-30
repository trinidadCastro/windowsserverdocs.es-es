---
title: rem
description: Artículo de referencia para el comando REM, que registra los comentarios en un script, un lote o un archivo de config.sys.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe0bfce3f9f72d0a32ef5b3bb540e5a297df24a1
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409706"
---
# <a name="rem"></a>rem

Registra los comentarios en un script, un lote o un archivo de config.sys. Si no se especifica ningún comentario, **REM** agrega espaciado vertical.

## <a name="syntax"></a>Sintaxis

```
rem [<comment>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<comment>` | Especifica una cadena de caracteres que se va a incluir como comentario. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- El comando **REM** no muestra comentarios en la pantalla. Para mostrar los comentarios en la pantalla, debe incluir el comando **echo on** en el archivo.

- No se puede usar un carácter de redirección ( `<` o `>` ) o una barra vertical ( `|` ) en un Comentario de archivo por lotes.

- Aunque puede usar **REM** sin un comentario para agregar espaciado vertical a un archivo por lotes, también puede usar líneas en blanco. Las líneas en blanco se omiten cuando se procesa un programa por lotes.

### <a name="examples"></a>Ejemplos

Para agregar espaciado vertical a través de los comentarios de archivo por lotes, escriba:

```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause
format b: /v chkdsk b:
```

Para incluir un comentario explicativo antes del comando **prompt** en un archivo de config.sys, escriba:

```
rem Set prompt to indicate current directory
prompt $p$g
```

Para proporcionar un Comentario sobre lo que hace un script, escriba:

```
rem The commands in this script set up 3 drives.
rem The first drive is a primary partition and is
rem assigned the letter D. The second and third drives
rem are logical partitions, and are assigned letters
rem E and F.
create partition primary size=2048
assign d:
create partition extended
create partition logical size=2048
assign e:
create partition logical
assign f:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)