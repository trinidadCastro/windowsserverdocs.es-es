---
title: bitsadmin getpriority
description: Artículo de referencia para el comando bitsadmin GetPriority (, que recupera la prioridad del trabajo especificado.
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 57d51e4a2a34fb5ae1361e864ee932ac314662b2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894032"
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
