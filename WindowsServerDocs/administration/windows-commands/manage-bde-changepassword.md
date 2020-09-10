---
title: Manage-BDE ChangePassword
description: Artículo de referencia para el comando Manage-BDE ChangePassword, que modifica la contraseña de una unidad de datos.
ms.topic: reference
ms.assetid: b174e152-8442-4fba-8b33-56a81ff4f547
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8c262b6ca123d76af6ebdca09e5546f41350686d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622706"
---
# <a name="manage-bde-changepassword"></a>Manage-BDE ChangePassword

Modifica la contraseña de una unidad de datos. Se solicita al usuario una nueva contraseña.

## <a name="syntax"></a>Sintaxis

```
manage-bde -changepassword [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -COMPUTERNAME | Especifica que se utilizará manage-bde.exe para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para cambiar la contraseña usada para desbloquear BitLocker en la unidad de datos D, escriba:

```
manage-bde –changepassword D:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)
