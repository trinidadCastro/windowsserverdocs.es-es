---
title: select vdisk
description: Artículo de referencia del comando SELECT vDisk, que selecciona el disco duro virtual (VHD) especificado y desplaza el foco a él.
ms.topic: reference
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 19e842115f914711cc59cfb770ee19f732b3a388
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389211"
---
# <a name="select-vdisk"></a>select vdisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Selecciona el VHD del disco duro virtual especificado \( \) y desplaza el foco a él.

## <a name="syntax"></a>Sintaxis

```
select vdisk file=<full path> [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| archivo =`<full path>` | Especifica la ruta de acceso completa y el nombre de un archivo VHD existente. |
| noerr | Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para desplazar el foco al VHD denominado *c:\test\test.vhd*, escriba:

```
select vdisk file=c:\test\test.vhd
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [attach vdisk](attach-vdisk.md)

- [compact vdisk](compact-vdisk.md)

- [detach vdisk](detach-vdisk.md)

- [detail vdisk](detail-vdisk.md)

- [expand vdisk](expand-vdisk.md)

- [merge vdisk](merge-vdisk.md)

- [list](list.md)

- [Seleccionar disco (comando)](select-disk.md)

- [seleccionar partición (comando)](select-partition.md)

- [comando Seleccionar volumen](select-volume.md)