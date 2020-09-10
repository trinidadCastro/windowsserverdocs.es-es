---
title: assign
description: Artículo de referencia del comando Assign, que asigna una letra de unidad o un punto de montaje al volumen que tiene el foco.
ms.topic: reference
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ff5f911fdaa4fc3703cbef2374b7431925a44fcf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633462"
---
# <a name="assign"></a>assign

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Asigna una letra de unidad o un punto de montaje al volumen que tiene el foco. También puede usar este comando para cambiar la letra de unidad asociada a una unidad extraíble. Si no se especifica una letra de unidad o un punto de montaje, se asigna la siguiente letra de unidad disponible. Si la letra de unidad o el punto de montaje ya están en uso, se genera un error.

Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

> [!IMPORTANT]
> No se pueden asignar Letras de unidad a volúmenes del sistema, volúmenes de arranque o volúmenes que contengan el archivo de paginación. Además, no se puede asignar una letra de unidad a una partición de fabricante de equipos originales (OEM) o a una partición de tabla de particiones GUID (GPT) que no sea una partición de datos básica.

## <a name="syntax"></a>Sintaxis

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `letter=<d>` | La letra de la unidad que desea asignar al volumen. |
| `mount=<path>` | La ruta de acceso del punto de montaje que desea asignar al volumen. Para obtener instrucciones sobre cómo usar este comando, consulte [asignar una ruta de carpeta de punto de montaje a una unidad](../../storage/disk-management/assign-a-mount-point-folder-path-to-a-drive.md). |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para asignar la letra E al volumen en el foco, escriba:

```
assign letter=e
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Seleccionar volumen](select-volume.md)
