---
title: set verbose
description: Artículo de referencia para el comando set verbose, que especifica si se proporciona una salida detallada durante la creación de la instantánea.
ms.topic: reference
ms.assetid: 93cb93c9-666f-4c74-814b-1c404a949935
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e0b4d4cca66b20125c1aa0bbb67a7806133f6d8
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389047"
---
# <a name="set-verbose"></a>Establecer detallado

Especifica si se proporciona la salida detallada durante la creación de la instantánea. Si se usa sin parámetros, **set verbose** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set verbose {on | off}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| on | Activa el registro de salida detallado durante el proceso de creación de la instantánea. Si el modo detallado está activado, **set** proporciona detalles de inclusión o exclusión del escritor y detalles de la compresión y extracción de metadatos. |
| apagado | Desactiva el registro de salida detallado durante el proceso de creación de la instantánea. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [establecer contexto (comando)](set-context.md)

- [comando establecer metadatos](set-metadata.md)

- [comando set option](set-option.md)
