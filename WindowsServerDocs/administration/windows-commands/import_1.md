---
title: importar
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 379d5923a9355db2965b56c27cedd207b1b63006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885996"
---
# <a name="import"></a>importar



Importa un grupo de discos externos en el grupo de discos del equipo local.

## <a name="syntax"></a>Sintaxis

```
import [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   El comando de importación importa todos los discos que se encuentra en el mismo grupo que el disco con el foco.
-   Debe seleccionarse un disco dinámico en un grupo de discos externos para que realizar esta operación correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para importar todos los discos que se encuentra en el mismo grupo de discos que el disco con el foco en el grupo de discos del equipo local, escriba:
```
import
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

