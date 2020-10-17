---
title: vssadmin list shadows
description: Una descripción del comando vssadmin List Shadows, que enumera todas las instantáneas existentes de un volumen especificado.
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: cb7ddaa09e2da350c1c5422c6224aabd8831f763
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156207"
---
# <a name="vssadmin-list-shadows"></a>vssadmin list shadows

> Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Muestra todas las instantáneas existentes de un volumen especificado. Si usa este comando sin parámetros, se muestran todas las instantáneas de volumen en el equipo en el orden indicado por el **conjunto de instantáneas**.

## <a name="syntax"></a>Sintaxis

```
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /for =`<ForVolumeSpec>` | Especifica en qué volumen se mostrarán las instantáneas. |
| /Shadow =`<ShadowID>` | Muestra la instantánea especificada por ShadowID. Para obtener el identificador de la instantánea, use el [comando vssadmin List Shadows](vssadmin-list-shadows.md). Cuando escriba un identificador de instantánea, use el formato siguiente, donde cada *X* representa un carácter hexadecimal:<p>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [vssadmin (comando)](vssadmin.md)

- [vssadmin List Shadows (comando)](vssadmin-list-shadows.md)
