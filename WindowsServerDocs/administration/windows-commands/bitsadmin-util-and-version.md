---
title: bitsadmin util y version
description: Tema de referencia del comando bitsadmin util and version, que muestra la versión del servicio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20c3db6e6fcd5ef3d00287f36c9f9624ab5224dd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707590"
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
