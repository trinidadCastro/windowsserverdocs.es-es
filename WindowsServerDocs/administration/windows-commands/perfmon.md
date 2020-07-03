---
title: perfmon
description: Artículo de referencia del comando perfmon, que inicia el monitor de confiabilidad y rendimiento de Windows en un modo independiente específico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8d5eca-8473-463e-a6e0-7bbd590b18e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/25/2018
ms.openlocfilehash: 5beaa1692f3fc4aa66eae81069f0ead5839d22e3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922831"
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

- [Monitor de rendimiento de Windows](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749154(v%3dws.11))
