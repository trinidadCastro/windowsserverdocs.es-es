---
title: nslookup set class
description: Artículo de referencia del comando Nslookup Set Class, que cambia la clase de consulta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 073f27f6f10721b11e6d0889d1cb8c16f15db283
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934425"
---
# <a name="nslookup-set-class"></a>nslookup set class

Cambia la clase de consulta. La clase especifica el grupo de protocolos de la información.

## <a name="syntax"></a>Sintaxis

```
set class=<class>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<class>` | Los valores válidos son:<ul><li>**En:** Especifica la clase de Internet. Este es el valor predeterminado.</li><li>**Caos:** Especifica la clase caos.</li><li>**HESIOD:** Especifica la clase Hesiod de la clase MIT Athena.</li><li>**Cualquiera:** Especifica que se debe utilizar cualquiera de los valores enumerados anteriormente.</li></ul> |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
