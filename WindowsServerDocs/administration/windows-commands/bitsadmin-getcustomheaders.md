---
title: bitsadmin getcustomheaders
description: Artículo de referencia para el comando bitsadmin getcustomheaders, que recupera los encabezados HTTP personalizados del trabajo.
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3cc970a7aeee1be678a3c77d852af91ae0ab98c8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894412"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders

Recupera los encabezados HTTP personalizados del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para obtener los encabezados personalizados para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
