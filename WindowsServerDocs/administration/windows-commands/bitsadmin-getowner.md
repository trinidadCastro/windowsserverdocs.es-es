---
title: bitsadmin getowner
description: Artículo de referencia para el comando bitsadmin GetOwner, que recupera el propietario del trabajo especificado.
ms.topic: reference
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1100ac3e126c1186ac498c0ee17831cbd2c6a694
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028713"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Muestra el nombre para mostrar o el GUID del propietario del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para mostrar el propietario del trabajo denominado *myDownloadJob*:

```
bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
