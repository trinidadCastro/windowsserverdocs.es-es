---
title: seleccionar comandos
description: Artículo de referencia para los comandos SELECT, que desplaza el foco a un disco, partición, volumen o disco duro virtual (VHD).
ms.topic: reference
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2b322fc7bf9355e64fbe14a0823c85dddadd6171
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389183"
---
# <a name="select-commands"></a>seleccionar comandos

Desplaza el foco a un disco, partición, volumen o disco duro virtual (VHD).

## <a name="syntax"></a>Sintaxis

```
select disk
select partition
select vdisk
select volume
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [Seleccionar disco](select-disk.md) | Desplaza el foco a un disco. |
| [Seleccionar partición](select-partition.md) | Desplaza el foco a una partición. |
| [Seleccionar vDisk](select-vdisk.md) | Desplaza el foco a un disco duro virtual. |
| [Seleccionar volumen](select-volume.md) | Desplaza el foco a un volumen. |

#### <a name="remarks"></a>Observaciones

- Si se selecciona un volumen con una partición correspondiente, la partición se seleccionará automáticamente.

- Si se selecciona una partición con un volumen correspondiente, el volumen se seleccionará automáticamente.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
