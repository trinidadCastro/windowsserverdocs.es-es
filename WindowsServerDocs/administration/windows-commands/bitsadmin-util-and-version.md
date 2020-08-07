---
title: bitsadmin util y version
description: Artículo de referencia del comando bitsadmin util and version, que muestra la versión del servicio BITS.
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3490fe4e3eaa217b81287d8a2ed38a6fdb98279
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880798"
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
