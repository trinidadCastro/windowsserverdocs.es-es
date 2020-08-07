---
title: bitsadmin resume
description: Artículo de referencia del comando bitsadmin resume, que activa un trabajo nuevo o suspendido en la cola de transferencia.
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a18bd6c0a69ff4b366f66d34ec472be9aaeecba2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893302"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Activa un trabajo nuevo o suspendido en la cola de transferencia. Si reanudó el trabajo por error, o simplemente necesita suspender el trabajo, puede usar el modificador de [suspensión de bitsadmin](bitsadmin-suspend.md) para suspender el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para reanudar el trabajo denominado *myDownloadJob*:

```
bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin comando suspender](bitsadmin-suspend.md)

- [bitsadmin (comando)](bitsadmin.md)
