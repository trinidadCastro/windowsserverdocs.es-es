---
title: bitsadmin listfiles
description: Artículo de referencia del comando bitsadmin ListFiles, que enumera los archivos del trabajo especificado.
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5dcd9092f2d9a8d150496e4cf89595537885d62
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893684"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles

Enumera los archivos del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /listfiles <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la lista de archivos del trabajo denominado *myDownloadJob*:

```
bitsadmin /listfiles myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
