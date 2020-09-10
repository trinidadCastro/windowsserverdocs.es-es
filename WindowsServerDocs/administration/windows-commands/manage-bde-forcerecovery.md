---
title: Manage-BDE forcerecovery
description: Artículo de referencia para el comando Manage-BDE forcerecovery, que fuerza una unidad protegida por BitLocker en modo de recuperación al reiniciar.
ms.topic: reference
ms.assetid: eecae37c-c9a3-46c5-b615-a0ace1f1d778
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b88aa4bb52ea6bcb50f34a6e8d42c6e14e0b1e59
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622613"
---
# <a name="manage-bde-forcerecovery"></a>Manage-BDE forcerecovery

Fuerza una unidad protegida por BitLocker en modo de recuperación al reiniciarse. Este comando elimina de la unidad todos los protectores de clave relacionados con el Módulo de plataforma segura (TPM). Cuando se reinicia el equipo, solo se puede usar una contraseña de recuperación o una clave de recuperación para desbloquear la unidad.

## <a name="syntax"></a>Sintaxis

```
manage-bde –forcerecovery <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para hacer que BitLocker se inicie en modo de recuperación en la unidad C, escriba:

```
manage-bde –forcerecovery C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)
