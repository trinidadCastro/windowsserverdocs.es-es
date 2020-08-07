---
title: bitsadmin getfilestransferred
description: Artículo de referencia para el comando bitsadmin getfilestransferred, que recupera el número de archivos transferidos para el trabajo especificado.
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c089dc8a122f558a396fdffb77b173b7586ee900
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894299"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

Recupera el número de archivos transferidos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el número de archivos transferidos en el trabajo denominado *myDownloadJob*:

```
bitsadmin /getfilestransferred myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
