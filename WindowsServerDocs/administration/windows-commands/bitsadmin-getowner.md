---
title: bitsadmin getowner
description: Artículo de referencia para el comando bitsadmin GetOwner, que recupera el propietario del trabajo especificado.
ms.topic: reference
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2839936113e304e9c57308042f5c8cf66e887d36
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631878"
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
