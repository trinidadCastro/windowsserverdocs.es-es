---
title: bitsadmin getmodificationtime
description: Artículo de referencia para el comando bitsadmin getmodificationtime, que recupera la última vez que se modificó el trabajo o cuando los datos se transfirieron correctamente.
ms.topic: reference
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 293b8ef47374c0df3f9b1cd3d76802d592c3c522
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631929"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime

Recupera la última vez que se modificó el trabajo o los datos se transfirieron correctamente.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getmodificationtime <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la hora de la última modificación del trabajo denominado *myDownloadJob*:

```
bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
