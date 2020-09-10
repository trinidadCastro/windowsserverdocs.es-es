---
title: Manage-BDE setidentifier
description: Artículo de referencia para el comando Manage-BDE setidentifier, que establece el campo de identificador de unidad de la unidad en el valor especificado en la opción proporcionar los identificadores únicos de la organización directiva de grupo.
ms.topic: reference
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 861eca94cb15c70857138b6f9c76b5dcf59089a9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634346"
---
# <a name="manage-bde-setidentifier"></a>Manage-BDE setidentifier

Establece el campo Identificador de unidad de la unidad en el valor especificado en la configuración **proporcionar los identificadores únicos de la organización** Directiva de grupo.

## <a name="syntax"></a>Sintaxis

```
manage-bde –setidentifier <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para establecer el campo de identificador de unidad BitLocker para C, escriba:

```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)

- [Guía de recuperación de BitLocker](/windows/security/information-protection/bitlocker/bitlocker-recovery-guide-plan)
