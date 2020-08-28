---
title: delete volume
description: Artículo de referencia del comando DELETE Volume, que elimina el volumen seleccionado.
ms.topic: reference
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c88834b800414bcf3ff246272ec187fb98d47db
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024159"
---
# <a name="delete-volume"></a>delete volume

Elimina el volumen seleccionado. Antes de comenzar, debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando [seleccionar volumen](select-volume.md) para seleccionar un volumen y cambiar el foco a él.

> [!IMPORTANT]
> No se puede eliminar el volumen del sistema, el volumen de arranque o cualquier volumen que contenga el archivo de paginación activo o el volcado de memoria (volcado de memoria).

## <a name="syntax"></a>Sintaxis

```
delete volume [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para eliminar el volumen que tiene el foco, escriba:

```
delete volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [select volume](select-volume.md)

- [eliminar comando](delete.md)