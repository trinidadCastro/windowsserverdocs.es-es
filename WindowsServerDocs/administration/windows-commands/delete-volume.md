---
title: delete volume
description: Tema de referencia de DELETE Volume, que elimina el volumen seleccionado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9a8ae0fc863cec5c1a3f6debccf8201e96badd0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716690"
---
# <a name="delete-volume"></a>delete volume

Elimina el volumen seleccionado.

## <a name="syntax"></a>Sintaxis

```
delete volume [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Observaciones

-   No puedes eliminar el volumen del sistema, el volumen de arranque o cualquier otro volumen que incluya el archivo de paginación activo o de volcado (volcado de memoria).
-   Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

## <a name="examples"></a>Ejemplos

Para eliminar el volumen que tiene el foco, escriba:
```
delete volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

