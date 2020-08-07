---
title: bitsadmin rawreturn
description: Artículo de referencia para el comando bitsadmin rawreturn, que devuelve los datos adecuados para el análisis.
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88b1e45d7fad2820a77d9f445065ae0ca182abb6
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893426"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Devuelve los datos adecuados para el análisis. Normalmente, este comando se usa junto con los modificadores **/Create** e **/Get*** para recibir solo el valor. Debe especificar este modificador antes que otros modificadores.

> [!NOTE]
> Este comando quita los caracteres de nueva línea y el formato de la salida.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /rawreturn
```

## <a name="examples"></a>Ejemplos

Para recuperar los datos sin procesar para el estado del trabajo denominado *myDownloadJob*:

```
bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
