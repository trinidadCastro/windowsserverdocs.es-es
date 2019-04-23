---
title: rem
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 85c8a69bf21a386cd36e45bbca6dacd35aef2509
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847006"
---
# <a name="rem"></a>rem



Registra los comentarios en un archivo por lotes o CONFIG. SYS. Si no se especifica ningún comentario, **rem** agrega espaciado vertical.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
rem [<Comment>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Comment>|Especifica una cadena de caracteres que se va a incluir como un comentario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **rem** comando no muestra los comentarios en la pantalla. Debe usar el **eco** comando en el lote o la configuración. Sys para mostrar los comentarios en la pantalla.
-   No se puede usar un carácter de redirección (**<** o **>**) o la canalización (**|**) en un comentario del archivo por lotes.
-   Aunque puede usar **rem** sin un comentario para agregar espacio vertical a un archivo por lotes, también puede usar las líneas en blanco. Líneas en blanco se omiten cuando se procesa un programa por lotes.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente muestra un archivo por lotes que utiliza los comentarios de comentarios y para el espaciado vertical:
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
Para incluir un comentario explicativo antes el **símbolo del sistema** comando en el archivo CONFIG. SYS archivo, agregue las líneas siguientes a la configuración. SYS:
```
rem Set prompt to indicate current directory
prompt $p$g
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)