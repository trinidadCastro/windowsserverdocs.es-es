---
title: bitsadmin getpriority
description: Tema de referencia del comando bitsadmin GetPriority (, que recupera la prioridad del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 38f92e83ccf5b048d168ce6a21c6026f490b18bf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717675"
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
