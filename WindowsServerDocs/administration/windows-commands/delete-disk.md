---
title: Eliminar disco
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e401133854118e82a45fd5e01466288ae41f67da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863916"
---
# <a name="delete-disk"></a>Eliminar disco



Elimina un disco dinámico que falta en la lista de discos.

Para obtener instrucciones sobre cómo usar este comando, consulte [quitar un disco dinámico que falta](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintaxis

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|
|invalidar|Permite que DiskPart elimine todos los volúmenes simples en el disco. Si el disco contiene la mitad de un volumen reflejado, se eliminará la mitad del reflejo en el disco. El comando delete disk override produce un error si el disco es un miembro de un volumen RAID-5.|

## <a name="BKMK_examples"></a>Ejemplos

Para eliminar un disco dinámico que falta en la lista de discos, escriba:
```
delete disk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

