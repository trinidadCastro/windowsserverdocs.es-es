---
title: resume de Manage-BDE
description: Artículo de referencia para el comando Manage-BDE resume, que reanuda el cifrado o descifrado de BitLocker después de que se haya pausado.
ms.topic: reference
ms.assetid: ca3cd1ca-6f2c-4190-b68f-27816635facb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ebae22e60a8110bc2ba5e4649fc6cf143431b359
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639292"
---
# <a name="manage-bde-resume"></a>resume de Manage-BDE

Reanuda el cifrado o descifrado de BitLocker después de que se haya pausado.

## <a name="syntax"></a>Sintaxis

```
manage-bde -resume [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para reanudar el cifrado de BitLocker en la unidad C, escriba:

```
manage-bde –resume C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Manage-BDE on (comando)](manage-bde-on.md)

- [comando Manage-BDE OFF](manage-bde-off.md)

- [comando Manage-BDE PAUSE](manage-bde-pause.md)

- [comando Manage-BDE](manage-bde.md)
