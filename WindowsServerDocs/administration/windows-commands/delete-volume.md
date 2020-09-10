---
title: delete volume
description: Artículo de referencia del comando DELETE Volume, que elimina el volumen seleccionado.
ms.topic: reference
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 66793e9d7c22b337164807bf76ee82c10d70547c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628804"
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