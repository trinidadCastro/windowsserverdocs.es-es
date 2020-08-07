---
title: bitsadmin getfilestotal
description: Artículo de referencia para el comando bitsadmin getfilestotal, que recupera el número de archivos del trabajo especificado.
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09867e5ed8b060f7a9cbfe573c6e98bfbac831df
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894336"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

Recupera el número de archivos del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el número de archivos incluidos en el trabajo denominado *myDownloadJob*:

```
bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>Consulte también

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
