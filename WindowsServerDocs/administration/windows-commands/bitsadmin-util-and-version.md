---
title: bitsadmin util y version
description: Artículo de referencia del comando bitsadmin util and version, que muestra la versión del servicio BITS.
ms.topic: reference
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f033f47f2a90d334512b9eb023eb6a7be44bcfc3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034673"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util y version

Muestra la versión del servicio BITS (por ejemplo, 2,0).

> [!NOTE]
> Este comando no es compatible con BITS 1,5 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /util /version [/verbose]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /verbose | Use este modificador para mostrar la versión de archivo de cada archivo DLL relacionado con BITS y comprobar si el servicio BITS se puede iniciar.|

## <a name="examples"></a>Ejemplos

Para mostrar la versión del servicio BITS.

```
bitsadmin /util /version
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin util (comando)](bitsadmin-util.md)

- [bitsadmin (comando)](bitsadmin.md)
