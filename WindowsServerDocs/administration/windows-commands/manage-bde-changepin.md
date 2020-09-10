---
title: Manage-BDE changepin
description: Artículo de referencia para el comando Manage-BDE changepin, que modifica el PIN de una unidad del sistema operativo.
ms.topic: reference
ms.assetid: c85aa1c7-3485-4839-a292-99dfcd6db252
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7b920e9c4580fced678e9d7dddd30fff66dad802
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622640"
---
# <a name="manage-bde-changepin"></a>Manage-BDE changepin

Modifica el PIN de una unidad del sistema operativo. Se solicita al usuario que escriba un nuevo PIN.

## <a name="syntax"></a>Sintaxis

```
manage-bde -changepin [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para cambiar el PIN que se usa con BitLocker en la unidad C, escriba:

```
manage-bde –changepin C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)
