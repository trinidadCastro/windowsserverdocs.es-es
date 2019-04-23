---
title: convertir en dinámico
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 353c1e4558ab2b0c948ec78c0cd87b579c738ec8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841616"
---
# <a name="convert-dynamic"></a>convertir en dinámico



Convierte un disco básico en un disco dinámico.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco básico a un disco dinámico](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Sintaxis

```
convert dynamic [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   Cualquier partición existente en el disco básico se convierten en volúmenes simples.
-   Debe seleccionarse un disco básico para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco básico y desplace el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para convertir un disco básico a un disco dinámico, escriba:
```
convert dynamic
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

