---
title: serverweroptin
description: Artículo de referencia para el comando serverweroptin, que permite activar los informes de errores.
ms.topic: reference
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c8a2fe32634704a22f12430b8516ed7f6cae4e82
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389138"
---
# <a name="serverweroptin"></a>serverweroptin

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Permite activar los informes de errores.

## <a name="syntax"></a>Sintaxis

```
serverweroptin [/query] [/detailed] [/summary]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /Query | Comprueba la configuración actual. |
| /detailed | Especifica que se envíen informes detallados automáticamente. |
| /Summary | Especifica que se envíen informes de Resumen de forma automática. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para comprobar la configuración actual, escriba:

```
serverweroptin /query
```

Para enviar automáticamente informes detallados, escriba:

```
serverweroptin /detailed
```

Para enviar automáticamente informes de Resumen, escriba:

```
serverweroptin /summary
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
