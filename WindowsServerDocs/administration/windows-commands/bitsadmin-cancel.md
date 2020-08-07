---
title: bitsadmin cancel
description: Artículo de referencia del comando bitsadmin CANCEL, que quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados con el trabajo.
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 603a398aef42052e25828b917748ed2b10287620
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894642"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Quita el trabajo de la cola de transferencia y elimina todos los archivos temporales asociados al trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para quitar el trabajo *myDownloadJob* de la cola de transferencia:

```
bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
