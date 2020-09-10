---
title: bitsadmin getnoprogresstimeout
description: Artículo de referencia del comando bitsadmin getnoprogresstimeout, que recupera el período de tiempo, en segundos, que el servicio intentará transferir el archivo después de que se produzca un error transitorio.
ms.topic: reference
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a7f5a84b90f124cb172ad551b196cac152a332a8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631912"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

Recupera el período de tiempo, en segundos, que el servicio intentará transferir el archivo después de que se produzca un error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el valor de tiempo de espera de progreso del trabajo denominado *myDownloadJob*:

```
bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
