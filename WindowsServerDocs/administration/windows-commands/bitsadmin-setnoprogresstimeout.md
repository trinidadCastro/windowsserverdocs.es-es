---
title: bitsadmin setnoprogresstimeout
description: Artículo de referencia para el comando bitsadmin setnoprogresstimeout, que establece el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produzca un error transitorio.
ms.topic: reference
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9ffe7280e6a27d1fbc8a95b6b4c8375a8df844f8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630808"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Establece el período de tiempo, en segundos, que BITS intenta transferir el archivo después de que se produzca el primer error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setnoprogresstimeout <job> <timeoutvalue>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| timeoutvalue | Período de tiempo durante el cual BITS espera para transferir un archivo después del primer error, en segundos. |

### <a name="remarks"></a>Observaciones

- El intervalo de tiempo de espera "no Progress" comienza cuando el trabajo encuentra su primer error transitorio.

- El intervalo de tiempo de espera se detiene o se restablece cuando se transfiere correctamente un byte de datos.

- Si el intervalo de tiempo de espera de "no Progress" supera el *timeoutvalue*, el trabajo se coloca en un estado de error irrecuperable.

## <a name="examples"></a>Ejemplos

Para establecer el valor de tiempo de espera de "no Progress" en 20 segundos, para el trabajo denominado *myDownloadJob*:

```
bitsadmin /setnoprogresstimeout myDownloadJob 20
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
