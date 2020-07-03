---
title: bitsadmin getcompletiontime
description: Artículo de referencia para el comando bitsadmin getcompletiontime, que recupera la hora en que el trabajo terminó de transferir los datos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e07dd6a345cd1bd58277ef08e08802a62d6e6772
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923102"
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
