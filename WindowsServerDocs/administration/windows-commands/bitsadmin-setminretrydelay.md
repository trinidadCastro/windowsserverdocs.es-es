---
title: bitsadmin setminretrydelay
description: Artículo de referencia para el comando bitsadmin setminretrydelay, que establece el tiempo mínimo, en segundos, que BITS espera después de encontrar un error transitorio antes de intentar transferir el archivo.
ms.topic: reference
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7ddce1aea607ed279aaa6c582ddc1661d269204e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630819"
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
