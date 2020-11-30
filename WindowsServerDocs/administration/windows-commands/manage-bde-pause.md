---
title: Manage-BDE-PAUSE
description: Artículo de referencia para el comando Manage-BDE-PAUSE, que pone en pausa el cifrado o descifrado de BitLocker.
ms.topic: reference
ms.assetid: efda0e08-b9ff-4e71-83d8-bb666b3032bd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b4f4255c544f6881ea7d023bbf03c92dc817ab34
ms.sourcegitcommit: 3181fcb69a368f38e0d66002e8bc6fd9628b1acc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96330386"
---
# <a name="manage-bde--pause"></a>Manage-BDE-PAUSE

Pausa el cifrado o descifrado de BitLocker.

## <a name="syntax"></a>Sintaxis

```
manage-bde -pause [<volume>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<volume>` | Especifica una letra de unidad seguida de dos puntos, una ruta de acceso de GUID de volumen o un volumen montado. |
| -COMPUTERNAME | Especifica que se utilizará manage-bde.exe para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para pausar el cifrado de BitLocker en la unidad C, escriba:

```Output
manage-bde -pause C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Manage-BDE on (comando)](manage-bde-on.md)

- [comando Manage-BDE OFF](manage-bde-off.md)

- [comando Manage-BDE resume](manage-bde-resume.md)

- [comando Manage-BDE](manage-bde.md)
