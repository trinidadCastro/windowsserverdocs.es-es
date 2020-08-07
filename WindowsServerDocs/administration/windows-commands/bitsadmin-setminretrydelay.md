---
title: bitsadmin setminretrydelay
description: Artículo de referencia para el comando bitsadmin setminretrydelay, que establece el tiempo mínimo, en segundos, que BITS espera después de encontrar un error transitorio antes de intentar transferir el archivo.
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 448e03805f30af50abffa28365e456028598ae1a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893078"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Establece la cantidad mínima de tiempo, en segundos, que BITS espera después de encontrar un error transitorio antes de intentar transferir el archivo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| retrydelay | Cantidad mínima de tiempo para que los BITS esperen después de un error durante la transferencia, en segundos. |

## <a name="examples"></a>Ejemplos

Para establecer el intervalo mínimo de reintentos en 35 segundos para el trabajo denominado *myDownloadJob*:

```
bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
