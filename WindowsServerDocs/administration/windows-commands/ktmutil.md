---
title: ktmutil
description: Artículo de referencia para el comando ktmutil, que inicia la utilidad Administrador de transacciones de kernel.
ms.topic: reference
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 07067415406a0f8ba88d24362ec1b6252f8b88cc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627592"
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