---
title: perfmon
description: Artículo de referencia del comando perfmon, que inicia el monitor de confiabilidad y rendimiento de Windows en un modo independiente específico.
ms.topic: reference
ms.assetid: 9a8d5eca-8473-463e-a6e0-7bbd590b18e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/25/2018
ms.openlocfilehash: 331a3e1550c2b3b5fcba47e5eddd2e58e5c5c23d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037623"
---
# <a name="perfmon"></a>perfmon

Inicie el monitor de confiabilidad y rendimiento de Windows en un modo independiente específico.

## <a name="syntax"></a>Sintaxis

```
perfmon </res|report|rel|sys>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /res | Inicia el Vista de recursos. |
| /Report | Inicia el conjunto de recopiladores de datos de diagnóstico del sistema y muestra un informe de los resultados. |
| /rel | Inicia el monitor de confiabilidad. |
| /sys | Inicia el monitor de rendimiento. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Monitor de rendimiento de Windows](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc749154(v%3dws.11))
