---
title: bitsadmin setvalidationstate
description: Artículo de referencia para el comando bitsadmin setvalidationstate, que establece el estado de validación del contenido del archivo especificado en el trabajo.
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dcdbd017f225704fc20d0472346d98fd84bb2c0
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881019"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate

Establece el estado de validación del contenido del archivo especificado en el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setvalidationstate <job> <file_index> <TRUE|FALSE>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| Trabajo | El nombre para mostrar o el GUID del trabajo. |
| file_index | Comienza en 0. |
| TRUE o FALSE | **True** activa la validación de contenido para el archivo especificado, mientras que **false** lo desactiva. |

## <a name="examples"></a>Ejemplos

Para establecer el estado de validación del contenido del archivo 2 en TRUE para el trabajo denominado *myDownloadJob*:

```
bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
