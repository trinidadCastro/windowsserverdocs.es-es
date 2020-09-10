---
title: nslookup set root
description: Artículo de referencia del comando Nslookup Set root, que cambia el nombre del servidor raíz que se usa para las consultas.
ms.topic: reference
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d7e1251b7e13320a77a4c59736a6bd985bd248af
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637480"
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
