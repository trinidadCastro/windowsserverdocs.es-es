---
title: delete volume
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d25ed68077f594c765cf5630648ad52528d8fe3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872516"
---
# <a name="delete-volume"></a>delete volume



Elimina el volumen seleccionado.

## <a name="syntax"></a>Sintaxis

```
delete volume [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   No puedes eliminar el volumen del sistema, el volumen de arranque o cualquier otro volumen que incluya el archivo de paginación activo o de volcado (volcado de memoria).
-   Debe seleccionarse un volumen para que esta operación se realice correctamente. Use la **seleccione volumen** comando para seleccionar un volumen y desplace el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para eliminar el volumen tiene el foco, escriba:
```
delete volume
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

