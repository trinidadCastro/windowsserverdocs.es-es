---
title: bitsadmin getcompletiontime
description: Artículo de referencia para el comando bitsadmin getcompletiontime, que recupera la hora en que el trabajo terminó de transferir los datos.
ms.topic: reference
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 660a28dcfe99bd5801ba2fc3ad909cec78a671ce
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632241"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

Recupera la hora en que el trabajo finalizó la transferencia de datos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la hora en que el trabajo llamado *myDownloadJob* terminó de transferir los datos:

```
bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
