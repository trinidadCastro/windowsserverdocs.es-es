---
title: 'bitsadmin getproxylist: recupera la lista de proxy para el trabajo especificado.'
description: Artículo de referencia para el comando bitsadmin getproxylist, que recupera la lista de proxy para el trabajo especificado.
ms.topic: reference
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4d4cefcb27a9aa18b06bc588d08aba2f2f810636
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631742"
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
