---
title: bitsadmin gethttpmethod
description: Artículo de referencia para el comando bitsadmin gethttpmethod, que obtiene el verbo HTTP que se va a usar con el trabajo.
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 5bbd2509cabea7ae68240a31cee78d9ffd3ac5e0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033533"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

Obtiene el verbo HTTP que se va a usar con el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el verbo HTTP que se va a usar con el trabajo denominado *myDownloadJob*:

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
