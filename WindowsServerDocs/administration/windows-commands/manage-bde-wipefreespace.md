---
title: Manage-BDE wipefreespace
description: Artículo de referencia para el comando Manage-BDE wipefreespace, que borra el espacio disponible en el volumen quitando los fragmentos de datos que puedan haber existido en el espacio.
ms.topic: reference
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ed8d67f223cd2946dca00d556797acad31685b8e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635211"
---
# <a name="manage-bde-wipefreespace"></a>Manage-BDE wipefreespace

Borra el espacio disponible en el volumen, quitando los fragmentos de datos que puedan haber existido en el espacio. La ejecución de este comando en un volumen cifrado mediante el método de cifrado **solo del espacio usado** proporciona el mismo nivel de protección que el método de cifrado del **cifrado de volumen completo** .

## <a name="syntax"></a>Sintaxis

```
manage-bde -wipefreespace|-w [<drive>] [-cancel] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -cancelar | Cancela un borrado de espacio disponible que está en curso. |
| -COMPUTERNAME | Especifica que se utilizará manage-bde.exe para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para borrar el espacio disponible en la unidad C, escriba: \

```
manage-bde -w C:
```

```
manage-bde -wipefreespace C:
```

Para cancelar el borrado del espacio libre en la unidad C, escriba:

```
manage-bde -w -cancel C:
```

```
manage-bde -wipefreespace -cancel C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)
