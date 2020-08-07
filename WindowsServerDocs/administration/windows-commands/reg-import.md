---
title: reg import
description: Artículo de referencia para el comando reg Import, que copia el contenido de un archivo que contiene subclaves del registro, entradas y valores exportados en el registro del equipo local.
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4461f10cb31447a40f3d49df7731980f0f8a88a2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884148"
---
# <a name="reg-import"></a>reg import

Copia el contenido de un archivo que contiene las subclaves del registro exportadas, las entradas y los valores en el registro del equipo local.

## <a name="syntax"></a>Sintaxis

```
reg import <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<filename>` | Especifica el nombre y la ruta de acceso del archivo que tiene el contenido que se va a copiar en el registro del equipo local. Este archivo debe crearse de antemano mediante **reg Export**. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los valores devueltos para la operación de **importación de reg** son:

    | Valor | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para importar las entradas del registro desde el archivo llamado CopiaAp. reg, escriba:

```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [REG Export (comando)](reg-export.md)
