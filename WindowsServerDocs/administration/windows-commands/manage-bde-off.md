---
title: Manage-BDE desactivado
description: Tema de referencia para el comando Manage-BDE OFF, que descifra la unidad y desactiva BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a27c119-d385-45e5-89fe-e311d4429876
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cbcec0cadba870a5f416af50f12e0b2c5eed6d95
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222864"
---
# <a name="manage-bde-off"></a>Manage-BDE desactivado

Descifra la unidad y desactiva BitLocker. Todos los protectores de clave se quitan cuando se completa el descifrado.

## <a name="syntax"></a>Sintaxis

```
manage-bde -off [<volume>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<volume>` | Especifica una letra de unidad seguida de dos puntos, una ruta de acceso de GUID de volumen o un volumen montado. |
| -COMPUTERNAME | Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para desactivar BitLocker en la unidad C, escriba:

```
manage-bde –off C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Manage-BDE on (comando)](manage-bde-on.md)

- [comando Manage-BDE PAUSE](manage-bde-pause.md)

- [comando Manage-BDE resume](manage-bde-resume.md)

- [comando Manage-BDE](manage-bde.md)
