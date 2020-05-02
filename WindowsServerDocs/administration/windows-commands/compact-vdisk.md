---
title: Compact vDisk
description: Tema de referencia para el comando Compact vDisk, que reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ae5c653645c9f6f3ef97501a59932682c24be3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82711149"
---
# <a name="compact-vdisk"></a>Compact vDisk

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
