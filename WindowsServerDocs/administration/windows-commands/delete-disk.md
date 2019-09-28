---
title: eliminar disco
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 886e84edf80227d42ad77e36aed388b9e853ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378670"
---
# <a name="delete-disk"></a>eliminar disco



Elimina un disco dinámico que falta de la lista de discos.

Para obtener instrucciones sobre cómo usar este comando, consulte [quitar un disco dinámico que falta](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintaxis

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|
|estima|Permite que DiskPart elimine todos los volúmenes simples del disco. Si el disco contiene la mitad de un volumen reflejado, se elimina la mitad del reflejo en el disco. Se produce un error en el comando DELETE Disk override si el disco es miembro de un volumen RAID-5.|

## <a name="BKMK_examples"></a>Example

Para eliminar un disco dinámico que falta en la lista de discos, escriba:
```
delete disk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

