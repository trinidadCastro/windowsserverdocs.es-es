---
title: delete disk
description: Comando comandos de Windows para eliminar disco, que elimina un disco dinámico que falta en la lista de discos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a767e0689d5fbabb193df37528a0909ab63a1ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846658"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para eliminar un disco dinámico que falta en la lista de discos, escriba:
```
delete disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

