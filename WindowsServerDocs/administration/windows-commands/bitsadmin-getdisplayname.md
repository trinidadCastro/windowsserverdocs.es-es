---
title: bitsadmin getdisplayname
description: Artículo de referencia del comando bitsadmin getDisplayName, que recupera el nombre para mostrar del trabajo especificado.
ms.topic: reference
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e106f9b1815d735502ee451ec59c7f34f9ea063
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632131"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

Recupera el nombre para mostrar del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el nombre para mostrar del trabajo denominado *myDownloadJob*:

```
bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
