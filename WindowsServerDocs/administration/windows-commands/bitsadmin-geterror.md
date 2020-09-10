---
title: bitsadmin geterror
description: Artículo de referencia para el comando bitsadmin getError, que recupera información de error detallada para el trabajo especificado.
ms.topic: reference
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1cfa4c5a7d0899e2d5eb34fd089944f2b801993a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632119"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

Recupera información de error detallada para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la información de error del trabajo denominado *myDownloadJob*:

```
bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
