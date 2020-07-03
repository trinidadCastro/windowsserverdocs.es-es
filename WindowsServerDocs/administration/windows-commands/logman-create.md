---
title: logman create
description: Artículo de referencia para el comando Logman Create, que crea un contador, un seguimiento, un recopilador de datos de configuración o una API.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 972f0126-7bc4-4b14-9265-062864f3ffd4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 695a101a0aa6a720b64ffee6617085d13b6e83d1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922327"
---
# <a name="logman-create"></a>logman create

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea un contador, un seguimiento, un recopilador de datos de configuración o una API.

## <a name="syntax"></a>Sintaxis

```
logman create <counter | trace | alert | cfg | api> <[-n] <name>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [logman create counter](logman-create-counter.md) | Crea un recopilador de datos de contador. |
| [logman create trace](logman-create-trace.md) | Crea un recopilador de datos de seguimiento. |
| [logman create alert](logman-create-alert.md) | Crea un recopilador de datos de alerta. |
| [logman create cfg](logman-create-cfg.md) | Crea un recopilador de datos de configuración. |
| [logman create api](logman-create-api.md) | Crea un recopilador de datos de seguimiento de API. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Logman (comando)](logman.md)