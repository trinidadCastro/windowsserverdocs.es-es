---
title: vssadmin resize shadowstorage
description: Una descripción del comando vssadmin Resize shadowstorage, que cambia el tamaño de la cantidad máxima de espacio de almacenamiento que se puede usar para el almacenamiento de instantáneas.
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 03/05/2020
ms.localizationpriority: medium
ms.openlocfilehash: 704130890d68c271e74a9163ba4ae76dcfb1a516
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156142"
---
# <a name="vssadmin-resize-shadowstorage"></a>vssadmin resize shadowstorage

> Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Cambia el tamaño de la cantidad máxima de espacio de almacenamiento que se puede usar para el almacenamiento de instantáneas.

La cantidad mínima de espacio de almacenamiento que se puede usar para el almacenamiento de instantáneas se puede especificar mediante el valor del registro **MinDiffAreaFileSize** . Para obtener más información, vea [MinDiffAreaFileSize](/windows/win32/backup/registry-keys-for-backup-and-restore#mindiffareafilesize).

> [!WARNING]
> Cambiar el tamaño de la Asociación de almacenamiento puede hacer desaparecer las instantáneas.

## <a name="syntax"></a>Sintaxis

```
vssadmin resize shadowstorage /for=<ForVolumeSpec> /on=<OnVolumeSpec> [/maxsize=<MaxSizeSpec>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /for =`<ForVolumeSpec>` | Especifica el volumen para el que se va a cambiar el tamaño máximo del espacio de almacenamiento. |
| /on =`<OnVolumeSpec>` | Especifica el volumen de almacenamiento. |
| [/MaxSize = `<MaxSizeSpec>` ] | Especifica la cantidad máxima de espacio que se puede utilizar para almacenar las instantáneas. Si no se especifica ningún valor para **/MaxSize**, no se aplica ningún límite en la cantidad de espacio de almacenamiento que se puede usar.<p>El valor de **tamañoMáxEspec** debe ser 1 MB o superior y debe expresarse en una de las siguientes unidades: KB, MB, GB, TB, PB o EB. Si no se especifica ninguna unidad, **tamañoMáxEspec** usa bytes de forma predeterminada. |

## <a name="examples"></a>Ejemplos

Para cambiar el tamaño de la instantánea del volumen C en el volumen D, con un tamaño máximo de 900 MB, escriba:

```
vssadmin resize shadowstorage /For=C: /On=D: /MaxSize=900MB
```

Para cambiar el tamaño de la instantánea del volumen C en el volumen D, sin tamaño máximo, escriba:

```
vssadmin resize shadowstorage /For=C: /On=D: /MaxSize=UNBOUNDED
```

Para cambiar el tamaño de la instantánea del volumen C en un 20%, escriba:

```
vssadmin resize shadowstorage /For=C: /On=C: /MaxSize=20%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [vssadmin (comando)](vssadmin.md)
