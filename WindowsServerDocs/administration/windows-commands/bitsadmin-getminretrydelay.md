---
title: bitsadmin getminretrydelay
description: Artículo de referencia para el comando bitsadmin getminretrydelay, que recupera el período de tiempo, en segundos, que el servicio espera después de encontrar un error transitorio antes de intentar transferir el archivo.
ms.topic: reference
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: be71dc355e966b4a6ac627045f1ec5ceaf68f2d3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631947"
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
