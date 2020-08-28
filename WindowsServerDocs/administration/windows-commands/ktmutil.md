---
title: ktmutil
description: Artículo de referencia para el comando ktmutil, que inicia la utilidad Administrador de transacciones de kernel.
ms.topic: reference
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bc69964d0fbf7ad93a91b16f78ed86515e537b3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028263"
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