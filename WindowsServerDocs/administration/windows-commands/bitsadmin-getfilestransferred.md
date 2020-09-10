---
title: bitsadmin getfilestransferred
description: Artículo de referencia para el comando bitsadmin getfilestransferred, que recupera el número de archivos transferidos para el trabajo especificado.
ms.topic: reference
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b4a34e799b2d7e41373a1b4682c77e3ad93d36c2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632091"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

Recupera el número de archivos transferidos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el número de archivos transferidos en el trabajo denominado *myDownloadJob*:

```
bitsadmin /getfilestransferred myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
