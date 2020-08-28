---
title: bitsadmin getnoprogresstimeout
description: Artículo de referencia del comando bitsadmin getnoprogresstimeout, que recupera el período de tiempo, en segundos, que el servicio intentará transferir el archivo después de que se produzca un error transitorio.
ms.topic: reference
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51a8327669bdadbebec28329e0c6b11cc32e672b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033523"
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
