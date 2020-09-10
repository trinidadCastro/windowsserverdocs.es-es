---
title: bitsadmin getbytestransferred
description: Artículo de referencia para el comando bitsadmin getbytestransferred, que recupera el número de bytes transferidos para el trabajo especificado.
ms.topic: reference
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c67185ff147611436dcb75803e2282542f376019
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632306"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

Recupera el número de bytes transferidos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el número de bytes transferidos para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getbytestransferred myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
