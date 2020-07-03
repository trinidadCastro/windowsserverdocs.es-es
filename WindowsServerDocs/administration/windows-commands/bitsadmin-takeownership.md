---
title: bitsadmin takeownership
description: Artículo de referencia para el comando bitsadmin TakeOwnerShip, que permite a un usuario con privilegios administrativos tomar posesión del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc406d1d5f7c9553082f85f3315ab7622f6bc7db
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927466"
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
