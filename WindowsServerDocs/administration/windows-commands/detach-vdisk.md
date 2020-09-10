---
title: detach vdisk
description: Artículo de referencia para el comando detach vDisk, que detiene el disco duro virtual (VHD) seleccionado para que aparezca como una unidad de disco duro local en el equipo host.
ms.topic: reference
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: af3608cfa3f92ecbda3c62401f5c5a85fa5a2ffe
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628768"
---
# <a name="detach-vdisk"></a>detach vdisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Detiene el disco duro virtual (VHD) seleccionado para que no aparezca como una unidad de disco duro local en el equipo host. Cuando se desasocia un VHD, puede copiarlo en otras ubicaciones. Antes de comenzar, debe seleccionar un disco duro virtual para que esta operación se realice correctamente. Use el comando [Select vDisk](select-vdisk.md) para seleccionar un disco duro virtual y desplazar el foco a él.


## <a name="syntax"></a>Sintaxis

```
detach vdisk [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para desasociar el disco duro virtual seleccionado, escriba:

```
detach vdisk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Attach vDisk (comando)](attach-vdisk.md)

- [Compact vDisk (comando)](compact-vdisk.md)

- [Detail vDisk (comando)](detail-vdisk.md)

- [comando Expand vDisk](expand-vdisk.md)

- [Merge vDisk (comando)](merge-vdisk.md)

- [comando SELECT vDisk](select-vdisk.md)

- [List (comando)](list.md)
