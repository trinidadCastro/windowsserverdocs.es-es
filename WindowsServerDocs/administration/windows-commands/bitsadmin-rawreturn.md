---
title: bitsadmin rawreturn
description: Artículo de referencia para el comando bitsadmin rawreturn, que devuelve los datos adecuados para el análisis.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b537b21678c100364406d4c59eaa02efd143e21
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926466"
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
