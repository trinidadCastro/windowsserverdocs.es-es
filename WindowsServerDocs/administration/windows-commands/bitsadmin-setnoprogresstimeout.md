---
title: bitsadmin setnoprogresstimeout
description: Artículo de referencia para el comando bitsadmin setnoprogresstimeout, que establece el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produzca un error transitorio.
ms.topic: reference
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc2559a963900234fd3111edb1a32e3f13b27e40
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027803"
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
