---
title: bitsadmin getvalidationstate
description: Artículo de referencia para el comando bitsadmin getvalidationstate, que notifica el estado de validación del contenido del archivo especificado en el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72d53572411a33259467fa8023b9ef08fe4a5595
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926622"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate

Notifica el estado de validación del contenido del archivo especificado en el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| file_index | Comienza en 0. |

## <a name="examples"></a>Ejemplos

Para recuperar el estado de validación del contenido del archivo 2 en el trabajo denominado *myDownloadJob*:

```
bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
