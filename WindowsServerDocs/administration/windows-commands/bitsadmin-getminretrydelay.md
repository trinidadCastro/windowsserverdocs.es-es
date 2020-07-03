---
title: bitsadmin getminretrydelay
description: Artículo de referencia para el comando bitsadmin getminretrydelay, que recupera el período de tiempo, en segundos, que el servicio espera después de encontrar un error transitorio antes de intentar transferir el archivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 066eb9a2c967d9d5e92aa8dbad2001a65a682796
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927019"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

Recupera el período de tiempo, en segundos, que el servicio esperará después de encontrar un error transitorio antes de intentar transferir el archivo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el intervalo mínimo de reintentos para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
