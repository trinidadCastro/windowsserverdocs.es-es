---
title: 'bitsadmin getproxylist: recupera la lista de proxy para el trabajo especificado.'
description: Artículo de referencia para el comando bitsadmin getproxylist, que recupera la lista de proxy para el trabajo especificado.
ms.topic: reference
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9038f9faaedce30bfdf6025a40e3f6369805ac2a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024419"
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
