---
title: delete disk
description: Tema de referencia de DELETE Disk, que elimina un disco dinámico que falta en la lista de discos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad4888835c0bb1862344f104099b8b59027d1de9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716755"
---
# <a name="delete-disk"></a>delete disk

Elimina un disco dinámico que falta de la lista de discos.

Para obtener instrucciones sobre cómo usar este comando, consulte [quitar un disco dinámico que falta](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintaxis

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|
|override|Permite que DiskPart elimine todos los volúmenes simples del disco. Si el disco contiene la mitad de un volumen reflejado, se eliminará la mitad del reflejo del disco. El comando delete disk override no funciona si el disco forma parte de un volumen RAID-5.|

## <a name="examples"></a>Ejemplos

Para eliminar un disco dinámico que falta en la lista de discos, escriba:
```
delete disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

