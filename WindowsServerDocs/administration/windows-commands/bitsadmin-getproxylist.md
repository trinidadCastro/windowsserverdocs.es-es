---
title: 'bitsadmin getproxylist: recupera la lista de proxy para el trabajo especificado.'
description: Tema de referencia del comando bitsadmin getproxylist, que recupera la lista de proxy para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a92703d83cc872204d3dc488c15d703dfd50a780
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717656"
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
