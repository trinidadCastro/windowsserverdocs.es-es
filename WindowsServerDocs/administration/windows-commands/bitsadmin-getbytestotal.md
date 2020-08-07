---
title: bitsadmin getbytestotal
description: Artículo de referencia para el comando bitsadmin getbytestotal, que recupera el tamaño del trabajo especificado.
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b26773578d4c9a24f969df1506828c202e12b41
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894562"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Recupera el tamaño del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el tamaño del trabajo denominado *myDownloadJob*:

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
