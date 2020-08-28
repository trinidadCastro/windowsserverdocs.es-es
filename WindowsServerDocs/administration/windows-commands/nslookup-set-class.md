---
title: nslookup set class
description: Artículo de referencia del comando Nslookup Set Class, que cambia la clase de consulta.
ms.topic: reference
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 676c9dad9d01c6889ec472d68df831ba3eec15c7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038786"
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
