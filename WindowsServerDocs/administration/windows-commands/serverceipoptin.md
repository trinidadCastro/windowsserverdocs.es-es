---
title: serverceipoptin
description: Artículo de referencia de serverceipoptin, que le permite participar en el Programa para la mejora de la experiencia del usuario (CEIP).
ms.topic: reference
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6df387158a9630ab767dda655346836355fae936
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389173"
---
# <a name="serverceipoptin"></a>serverceipoptin

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Permite participar en el Programa para la mejora de la experiencia del usuario (CEIP).

## <a name="syntax"></a>Sintaxis

```
serverceipoptin [/query] [/enable] [/disable]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /Query | Comprueba la configuración actual. |
| /Enable | Activa su participación en CEIP. |
| /Disable | Desactiva su participación en el CEIP. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para comprobar la configuración actual, escriba:

```
serverceipoptin /query
```

Para activar su participación, escriba:

```
serverceipoptin /enable
```

Para desactivar su participación, escriba:

```
serverceipoptin /disable
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
