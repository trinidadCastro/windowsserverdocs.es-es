---
title: bitsadmin geterror
description: Artículo de referencia para el comando bitsadmin getError, que recupera información de error detallada para el trabajo especificado.
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae0b2391267ddc1508d8343b909b9739a0e01ffb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894360"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

Recupera información de error detallada para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la información de error del trabajo denominado *myDownloadJob*:

```
bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
