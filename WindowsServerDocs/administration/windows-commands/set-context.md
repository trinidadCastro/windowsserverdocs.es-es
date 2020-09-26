---
title: Establecer contexto
description: Artículo de referencia para el comando set context, que establece el contexto para la creación de instantáneas.
ms.topic: reference
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ce1414ab90f116f58618fcd67bc203f25dc58e46
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389099"
---
# <a name="set-context"></a>Establecer contexto

Establece el contexto para la creación de la instantánea. Si se usa sin parámetros, **set context** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| clientaccessible | Especifica que las versiones de cliente de Windows pueden usar la instantánea. Este contexto es persistente de forma predeterminada. |
| permanentes | Especifica que la instantánea se mantiene entre la salida del programa, el restablecimiento o el reinicio. |
| volatile | Elimina la instantánea al salir o restablecer. |
| nowriters | Especifica que todos los escritores están excluidos. |

## <a name="examples"></a>Ejemplos

Para evitar que se eliminen las instantáneas al salir de DiskShadow, escriba:

```
set context persistent
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando establecer metadatos](set-metadata.md)

- [comando set option](set-option.md)

- [Set verbose (comando)](set-verbose.md)
