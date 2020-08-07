---
title: bitsadmin getbytestransferred
description: Artículo de referencia para el comando bitsadmin getbytestransferred, que recupera el número de bytes transferidos para el trabajo especificado.
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d6b8ebb8d03a2498796325de8878840f36c6b71
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894524"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

Recupera el número de bytes transferidos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el número de bytes transferidos para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getbytestransferred myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
