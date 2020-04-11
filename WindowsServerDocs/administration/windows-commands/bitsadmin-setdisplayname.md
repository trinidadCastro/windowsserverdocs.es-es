---
title: bitsadmin setdisplayname
description: Windows Commands topic for **bitsadmin setDisplayName**, que establece el nombre para mostrar del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b1086903dd130392800f325c451bb4750fbf8fa
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123007"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Establece el nombre para mostrar del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setdisplayname <job> <display_name>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| display_name | Texto que se usa como nombre que se muestra para el trabajo específico. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establece el nombre para mostrar del trabajo en *myDownloadJob*.

```
C:\>bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)