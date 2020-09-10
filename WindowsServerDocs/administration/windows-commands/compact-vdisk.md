---
title: compact vdisk
description: Artículo de referencia para el comando Compact vDisk, que reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica.
ms.topic: reference
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 459ffb1940d6e4b7fcce5c3fa97a89c5c938b205
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629410"
---
# <a name="compact-vdisk"></a>compact vdisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica. Este parámetro es útil porque los VHD de expansión dinámica aumentan de tamaño a medida que se agregan archivos, pero no se reducen automáticamente al eliminar archivos.

## <a name="syntax"></a>Sintaxis

```
compact vdisk
```

### <a name="remarks"></a>Observaciones

- Se debe seleccionar un VHD de expansión dinámica para que esta operación se realice correctamente. Use el [comando SELECT vDisk](select-vdisk.md) para seleccionar un disco duro virtual y desplazar el foco a él.

- Solo puede usar discos duros virtuales de expansión dinámica compacta que estén desasociados o adjuntados como de solo lectura.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Attach vDisk (comando)](attach-vdisk.md)

- [Detail vDisk (comando)](detail-vdisk.md)

- [Detach vDisk (comando)](detach-vdisk.md)

- [comando Expand vDisk](expand-vdisk.md)

- [Merge vDisk (comando)](merge-vdisk.md)

- [comando SELECT vDisk](select-vdisk.md)

- [List (comando)](list.md)
