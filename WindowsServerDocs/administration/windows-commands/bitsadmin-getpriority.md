---
title: bitsadmin getpriority
description: Artículo de referencia para el comando bitsadmin GetPriority (, que recupera la prioridad del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 39ce769e2eb1dcf7a05d416dc2a66c6c082c9ae4
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926859"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

Recupera la prioridad del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

#### <a name="output"></a>Output

La prioridad devuelta para este comando puede ser:

- **PLANOS**

- **CALIDAD**

- **SIN**

- **HABILITA**

- **UNKNOWN**

## <a name="examples"></a>Ejemplos

Para recuperar la prioridad del trabajo denominado *myDownloadJob*:

```
bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
