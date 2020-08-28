---
title: 'administrar: estado de BDE'
description: Artículo de referencia para el comando Manage-BDE status, que proporciona información acerca de todas las unidades del equipo, independientemente de si están protegidas con BitLocker.
ms.topic: reference
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 632e286b15d65c066a6f2229b98e12a23014f998
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027483"
---
# <a name="manage-bde-status"></a>administrar: estado de BDE

Proporciona información acerca de todas las unidades del equipo; tanto si están protegidos con BitLocker como si no, entre los que se incluyen:

- Size

- Versión de BitLocker

- Estado de la conversión

- Porcentaje cifrado

- Encryption method

- Estado de protección

- Estado de bloqueo

- Campo de identificación

- Protectores de clave

## <a name="syntax"></a>Sintaxis

```
manage-bde -status [<drive>] [-protectionaserrorlevel] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -protectionaserrorlevel | Hace que la herramienta de línea de comandos Manage-BDE envíe el código de retorno **0** si el volumen está protegido y **1** si el volumen está desprotegido; se usa normalmente para los scripts por lotes para determinar si una unidad está protegida por BitLocker. También puede usar **-p** como una versión abreviada de este comando. |
| -COMPUTERNAME | Especifica que se utilizará manage-bde.exe para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para mostrar el estado de la unidad C, escriba:

```
manage-bde –status C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)
