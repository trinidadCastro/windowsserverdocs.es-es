---
title: remove
description: Artículo de referencia del comando Remove, que quita una letra de unidad o un punto de montaje de un volumen.
ms.topic: reference
ms.assetid: b0886140-da8b-4231-8cb2-f280874d99c0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ba7d625c5908af4a209266293495e6d472cb730b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641036"
---
# <a name="remove"></a>remove

Quita una letra de unidad o un punto de montaje del volumen que tiene el foco. Si se utiliza el parámetro all, se quitan todas las letras de unidad y puntos de montaje actuales. Si no se especifica ninguna letra de unidad o punto de montaje, DiskPart quita la primera letra de unidad o punto de montaje que encuentra.

El comando Remove también se puede usar para cambiar la letra de unidad asociada a una unidad extraíble. No se pueden quitar las letras de unidad de los volúmenes de sistema, de arranque o de paginación. Además, no se puede quitar la letra de unidad de una partición OEM, ninguna partición GPT con un GUID no reconocido, ni ninguna de las particiones GPT especiales que no son de datos, como la partición del sistema EFI.

> [!NOTE]
> Se debe seleccionar un volumen para que el comando **quitar** se ejecute correctamente. Use el comando [seleccionar volumen](select-volume.md) para seleccionar un disco y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
remove [{letter=<drive> | mount=<path> [all]}] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| letra =`<drive>` | Letra de unidad que se va a quitar. |
| montaje =`<path>` | Ruta de acceso del punto de montaje que se va a quitar. |
| todo | Quita todas las letras de unidad y puntos de montaje actuales. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

### <a name="examples"></a>Ejemplos

Para quitar el d:\ , escriba:

```
remove letter=d
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
