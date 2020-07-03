---
title: bitsadmin setdescription
description: Artículo de referencia para el comando bitsadmin setDescription, que establece la descripción del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c22909193667ff3222576010498d7b2fbcf14c7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927830"
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

Para recuperar la descripción del trabajo denominado *myDownloadJob*:

```
bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
