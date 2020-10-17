---
title: vssadmin delete shadows
description: Una descripción del comando vssadmin Delete Shadows, que elimina las instantáneas del volumen especificado.
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2dcd1bf8cbe946c77d0b5a100d2a29ecb36ef50f
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156225"
---
# <a name="vssadmin-delete-shadows"></a>vssadmin delete shadows

> Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Elimina las instantáneas del volumen especificado. Solo se pueden eliminar instantáneas con el tipo *accesible desde el cliente* .

## <a name="syntax"></a>Sintaxis

```
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /for =`<ForVolumeSpec>` | Especifica la instantánea del volumen que se eliminará. |
| /oldest | Elimina solo la instantánea más antigua. |
| /all | Elimina todas las instantáneas del volumen especificado. |
| /Shadow =`<ShadowID>` | Elimina la instantánea especificada por ShadowID. Para obtener el identificador de la instantánea, use el [comando vssadmin List Shadows](vssadmin-list-shadows.md). Al escribir un identificador de instantánea, use el formato siguiente, donde cada *X* representa un carácter hexadecimal:<p>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX |
| /quiet | Especifica que el comando no mostrará mensajes mientras se ejecuta. |

## <a name="examples"></a>Ejemplos

Para eliminar la instantánea más antigua del volumen C, escriba:

```
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [vssadmin (comando)](vssadmin.md)

- [vssadmin List Shadows (comando)](vssadmin-list-shadows.md)
