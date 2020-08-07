---
title: ktmutil
description: Artículo de referencia para el comando ktmutil, que inicia la utilidad Administrador de transacciones de kernel.
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff65e7e228f4876e51e3132d3a430ceec3837053
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887668"
---
# <a name="ktmutil"></a>ktmutil

Inicia la utilidad Administrador de transacciones de kernel. Si se usa sin parámetros, **ktmutil** muestra los subcomandos disponibles.

## <a name="syntax"></a>Sintaxis

```
ktmutil list tms
ktmutil list transactions [{TmGUID}]
ktmutil resolve complete {TmGUID} {RmGUID} {EnGUID}
ktmutil resolve commit {TxGUID}
ktmutil resolve rollback {TxGUID}
ktmutil force commit {GUID}
ktmutil force rollback {GUID}
ktmutil forget
```

## <a name="examples"></a>Ejemplos


Para forzar una transacción dudosa con GUID 311a9209-03f4-11dc-918f-00188b8f707b para confirmar, escriba:

```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)