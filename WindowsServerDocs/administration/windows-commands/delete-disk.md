---
title: delete disk
description: Tema de referencia del comando DELETE Disk, que elimina un disco dinámico que falta en la lista de discos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c8076f486251e428bce8805e15c2aa74caaf834
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993143"
---
# <a name="delete-disk"></a>delete disk

Elimina un disco dinámico que falta de la lista de discos.

> [!NOTE]
> Para obtener instrucciones detalladas sobre cómo usar este comando, consulte [quitar un disco dinámico que falta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753029(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |
| override | Permite que DiskPart elimine todos los volúmenes simples del disco. Si el disco contiene la mitad de un volumen reflejado, se eliminará la mitad del reflejo del disco. El comando delete disk override no funciona si el disco forma parte de un volumen RAID-5. |

## <a name="examples"></a>Ejemplos

Para eliminar un disco dinámico que falta en la lista de discos, escriba:

```
delete disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [eliminar comando](delete.md)
