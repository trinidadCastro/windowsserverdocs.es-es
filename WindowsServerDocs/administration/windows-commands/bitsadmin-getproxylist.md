---
title: 'bitsadmin getproxylist: recupera la lista de proxy para el trabajo especificado.'
description: Artículo de referencia para el comando bitsadmin getproxylist, que recupera la lista de proxy para el trabajo especificado.
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bcd94c65a11006a795f071224397d8b3081b7548
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893979"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera la lista delimitada por comas de los servidores proxy que se usarán para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la lista de servidores proxy para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
