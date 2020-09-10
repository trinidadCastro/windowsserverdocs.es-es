---
title: bitsadmin gethttpmethod
description: Artículo de referencia para el comando bitsadmin gethttpmethod, que obtiene el verbo HTTP que se va a usar con el trabajo.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 0d96c85aa99330b133954a77ca5584fe2d02cd43
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632038"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

Obtiene el verbo HTTP que se va a usar con el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el verbo HTTP que se va a usar con el trabajo denominado *myDownloadJob*:

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
