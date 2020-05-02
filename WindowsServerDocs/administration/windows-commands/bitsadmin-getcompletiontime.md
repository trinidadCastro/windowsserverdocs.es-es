---
title: bitsadmin getcompletiontime
description: Tema de referencia del comando bitsadmin getcompletiontime, que recupera la hora en que el trabajo terminó de transferir los datos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b3721401e450ae60fb77534f8eb845ff5ac3443
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718115"
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
