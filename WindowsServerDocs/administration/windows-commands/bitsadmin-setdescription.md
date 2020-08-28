---
title: bitsadmin setdescription
description: Artículo de referencia para el comando bitsadmin setDescription, que establece la descripción del trabajo especificado.
ms.topic: reference
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86f63a553b9d308ef3e8bfe5bfc2a2334b5d28e8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031263"
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
