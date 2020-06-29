---
title: perfmon
description: Tema de referencia del comando perfmon, que inicia el monitor de confiabilidad y rendimiento de Windows en un modo independiente específico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8d5eca-8473-463e-a6e0-7bbd590b18e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/25/2018
ms.openlocfilehash: 96d1589dcd75814c37c2ad295cf60887eb07739c
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472481"
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
