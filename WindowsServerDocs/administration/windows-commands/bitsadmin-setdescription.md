---
title: bitsadmin setdescription
description: Windows Commands topic for **bitsadmin setDescription**, que establece la descripción del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b62e6b030c23c475418cd6f2c63f04edba1acff
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123014"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

Establece la descripción del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setdescription <job> <description>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| description | Texto que se usa para describir el trabajo. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se recupera la descripción del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)