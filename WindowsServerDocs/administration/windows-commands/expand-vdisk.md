---
title: expandir vDisk
description: Tema de referencia del comando Expand vDisk, que expande un disco duro virtual (VHD) hasta un tamaño especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48973178f35f792b52fa81e5ed59449ca5db2559
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436190"
---
# <a name="expand-vdisk"></a>expandir vDisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Expande un disco duro virtual (VHD) hasta un tamaño especificado.

Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el [comando SELECT vDisk](select-vdisk.md) para seleccionar un volumen y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
expand vdisk maximum=<n>
```

### <a name="parameters"></a>Parámetros

 | Parámetro | Descripción |
 |---------- | ----------- |
 | máximo =`<n>` | Especifica el nuevo tamaño del disco duro virtual en megabytes (MB). |

### <a name="examples"></a>Ejemplos

Para expandir el disco duro virtual seleccionado a 20 GB, escriba:

```
expand vdisk maximum=20000
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando SELECT vDisk](select-vdisk.md)

- [Attach vDisk (comando)](attach-vdisk.md)

- [Compact vDisk (comando)](compact-vdisk.md)

- [detach vDisk (comando)](detach-vdisk.md)

- [Detail vDisk (comando)](detail-vdisk.md)

- [Merge vDisk (comando)](merge-vdisk.md)

- [List (comando)](list.md)
