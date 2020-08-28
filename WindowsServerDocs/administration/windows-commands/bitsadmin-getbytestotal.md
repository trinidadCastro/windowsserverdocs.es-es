---
title: bitsadmin getbytestotal
description: Artículo de referencia para el comando bitsadmin getbytestotal, que recupera el tamaño del trabajo especificado.
ms.topic: reference
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a8496234c0907061997ddd790e03f78d84c5380
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024429"
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
