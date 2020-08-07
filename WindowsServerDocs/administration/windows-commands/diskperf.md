---
title: diskperf
description: Artículo de referencia para el comando diskperf, que se puede usar para habilitar o deshabilitar de forma remota los contadores de rendimiento de disco físico o lógico en equipos que ejecutan Windows.
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81cefe217abaa7b2d4ee843f3887076f66484422
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890870"
---
# <a name="diskperf"></a>diskperf

El comando **diskperf** habilita o deshabilita de forma remota los contadores de rendimiento de disco físico o lógico en equipos que ejecutan Windows.

## <a name="syntax"></a>Sintaxis

```
diskperf [-y[d|v] | -n[d|v]] [\\computername]
```

## <a name="options"></a>Opciones

| Opción | Descripción |
| ------ | ----------- |
| -y | Inicia todos los contadores de rendimiento de disco cuando se reinicia el equipo. |
| -yd | Habilita los contadores de rendimiento de disco para las unidades físicas cuando se reinicia el equipo. |
| -yv | Habilita los contadores de rendimiento de disco para unidades lógicas o volúmenes de almacenamiento cuando se reinicia el equipo. |
| -n | Deshabilita todos los contadores de rendimiento de disco cuando se reinicia el equipo. |
| -ND | Deshabilite los contadores de rendimiento de disco para las unidades físicas cuando se reinicie el equipo. |
| -NV | Deshabilite los contadores de rendimiento de disco para unidades lógicas o volúmenes de almacenamiento cuando se reinicie el equipo. |
| `\\<computername>` | Especifica el nombre del equipo en el que desea habilitar o deshabilitar los contadores de rendimiento del disco. |
| -? | Muestra la ayuda contextual. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
