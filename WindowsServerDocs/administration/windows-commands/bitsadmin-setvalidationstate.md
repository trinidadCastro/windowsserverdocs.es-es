---
title: bitsadmin setvalidationstate
description: Artículo de referencia para el comando bitsadmin setvalidationstate, que establece el estado de validación del contenido del archivo especificado en el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae776279b742e3af5af3cf555765007bbf0eb8de
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927496"
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
