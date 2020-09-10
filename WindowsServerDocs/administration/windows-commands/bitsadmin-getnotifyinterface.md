---
title: bitsadmin getnotifyinterface
description: Artículo de referencia del comando bitsadmin getnotifyinterface, que determina si otro programa ha registrado una interfaz de devolución de llamada COM para el trabajo especificado.
ms.topic: reference
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 563cdf103ce60fc13f0455caea98e30f9e2e03e4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631891"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina si otro programa ha registrado una interfaz de devolución de llamada COM (la interfaz de notificación) para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getnotifyinterface <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

#### <a name="output"></a>Resultados

La salida de este comando muestra, **registrado** o **anulado el registro**.

> [!NOTE]
> No es posible determinar el programa que registró la interfaz de devolución de llamada.

## <a name="examples"></a>Ejemplos

Para recuperar la interfaz Notify del trabajo denominado *myDownloadJob*:

```
bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
