---
title: diskperf
description: Artículo de referencia para el comando diskperf, que se puede usar para habilitar o deshabilitar de forma remota los contadores de rendimiento de disco físico o lógico en equipos que ejecutan Windows.
ms.topic: reference
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09f095f5e6184b7961d02c05da4c2ca0c56a0482
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034123"
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
