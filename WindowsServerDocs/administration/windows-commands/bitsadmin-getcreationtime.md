---
title: bitsadmin getcreationtime
description: Artículo de referencia para el comando bitsadmin GetCreationTime, que recupera la hora de creación del trabajo especificado.
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42cd6de0769d4a741e76a1f6b03c32123ea8cf6a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894456"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

Recupera la hora de creación del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la hora de creación del trabajo denominado *myDownloadJob*:

```
bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
