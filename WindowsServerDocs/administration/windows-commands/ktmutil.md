---
title: ktmutil
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e65312ea4bb3169b90c2550b8b945919b86587f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841198"
---
# <a name="ktmutil"></a>ktmutil



Inicia la utilidad Administrador de transacciones de kernel. Si se usa sin parámetros, **ktmutil** muestra los subcomandos disponibles.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

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

## <a name="remarks"></a>Comentarios

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para forzar una transacción dudosa con GUID 311a9209-03f4-11dc-918f-00188b8f707b para confirmar, escriba:
```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)