---
title: 'bitsadmin getproxylist: recupera la lista de proxy para el trabajo especificado.'
description: Artículo de referencia para el comando bitsadmin getproxylist, que recupera la lista de proxy para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60c66db909b9b62d33ffda09e3b696362b6a65a0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926817"
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
