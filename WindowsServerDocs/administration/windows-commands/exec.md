---
title: exec
description: Artículo de referencia para el comando exec, que ejecuta un archivo de script en el equipo local.
ms.topic: reference
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 668dc3cf3d93d11066d7dece4309f586b3a5b964
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635959"
---
# <a name="exec"></a>exec

Ejecuta un archivo de script en el equipo local. Este comando también duplica o restaura datos como parte de una secuencia de copia de seguridad o restauración. Si se produce un error en el script, se devuelve un error y se cierra DiskShadow.

El archivo puede ser un script **cmd** .

## <a name="syntax"></a>Sintaxis

```
exec <scriptfile.cmd>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<scriptfile.cmd>` | Especifica el archivo de script que se va a ejecutar. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [DiskShadow (comando)](diskshadow.md)
