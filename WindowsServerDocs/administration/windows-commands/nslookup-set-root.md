---
title: nslookup set root
description: Artículo de referencia del comando Nslookup Set root, que cambia el nombre del servidor raíz que se usa para las consultas.
ms.topic: reference
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b59f54445266bd1b4c12b04cf8011fc6a2321b9
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025169"
---
# <a name="nslookup-set-root"></a>nslookup set root

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el nombre del servidor raíz que se usa para las consultas.

> [!NOTE]
> Este comando admite el comando [nslookup root](nslookup-root.md) .

## <a name="syntax"></a>Sintaxis

```
set root=<rootserver>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ---------- | ---------- |
| `<rootserver>` | Especifica el nuevo nombre del servidor raíz. El valor predeterminado es **NS.NIC.DDN.mil**. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [nslookup root](nslookup-root.md)
