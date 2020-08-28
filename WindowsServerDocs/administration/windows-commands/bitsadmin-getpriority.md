---
title: bitsadmin getpriority
description: Artículo de referencia para el comando bitsadmin GetPriority (, que recupera la prioridad del trabajo especificado.
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 2aeff973b0ca285cc8c9852f284e314879f8de02
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028703"
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
