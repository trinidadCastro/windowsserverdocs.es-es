---
title: bitsadmin getfilestotal
description: Artículo de referencia para el comando bitsadmin getfilestotal, que recupera el número de archivos del trabajo especificado.
ms.topic: reference
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73a543914f31a7b9e5b028caf273d7945a954fdd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632104"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

Recupera el número de archivos del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el número de archivos incluidos en el trabajo denominado *myDownloadJob*:

```
bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>Consulte también

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
