---
title: bitsadmin takeownership
description: Artículo de referencia para el comando bitsadmin TakeOwnerShip, que permite a un usuario con privilegios administrativos tomar posesión del trabajo especificado.
ms.topic: reference
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0df40e336ecc282e4b1a1774d7b5848f37a1a7a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033383"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Permite a un usuario con privilegios administrativos tomar posesión del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para tomar posesión del trabajo denominado *myDownloadJob*:

```
bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
