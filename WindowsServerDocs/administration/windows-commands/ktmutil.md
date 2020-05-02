---
title: ktmutil
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47b447165ee54e6839bb6338801c6703d818caa8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724524"
---
# <a name="ktmutil"></a>ktmutil



Inicia la utilidad Administrador de transacciones de kernel. Si se usa sin parámetros, **ktmutil** muestra los subcomandos disponibles.



## <a name="syntax"></a>Sintaxis

```
ktmutil list tms 
ktmutil list transactions [{TmGuid}]
ktmutil resolve complete {TmGuid} {RmGuid} {EnGuid}
ktmutil resolve commit {TxGuid}
ktmutil resolve rollback {TxGuid}
ktmutil force commit {??Guid}
ktmutil force rollback {??Guid}
ktmutil forget
```

### <a name="parameters"></a>Parámetros

## <a name="remarks"></a>Observaciones

## <a name="examples"></a>Ejemplos

Para forzar una transacción dudosa con GUID 311a9209-03f4-11dc-918f-00188b8f707b para confirmar, escriba:
```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)