---
title: Manage-BDE AUTOLOCK
description: Artículo de referencia para el comando Manage-BDE AUTOLOCK, que administra el desbloqueo automático de unidades de datos protegidas por BitLocker.
ms.topic: reference
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e6035b9d4a851631770b01ba525759d1d61992f5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633751"
---
# <a name="manage-bde-autounlock"></a>Manage-BDE AUTOLOCK

Administra el desbloqueo automático de las unidades de datos protegidas por BitLocker.

## <a name="syntax"></a>Sintaxis

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -enable | Habilita el desbloqueo automático para una unidad de datos. |
| -disable | Deshabilita el desbloqueo automático para una unidad de datos. |
| -clearallkeys | Quita todas las claves externas almacenadas en la unidad del sistema operativo. |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -COMPUTERNAME | Especifica que se utilizará manage-bde.exe para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para habilitar el desbloqueo automático de la unidad de datos E, escriba:

```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)