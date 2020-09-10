---
title: bitsadmin setdisplayname
description: Artículo de referencia para el comando bitsadmin setDisplayName, que establece el nombre para mostrar del trabajo especificado.
ms.topic: reference
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 57a72d198b7d262f3a7958920e5d54955f8b6270
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630938"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Establece el nombre para mostrar del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setdisplayname <job> <display_name>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| display_name | Texto que se usa como nombre que se muestra para el trabajo específico. |

## <a name="examples"></a>Ejemplos

Para establecer el nombre para mostrar del trabajo en *myDownloadJob*:

```
bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
