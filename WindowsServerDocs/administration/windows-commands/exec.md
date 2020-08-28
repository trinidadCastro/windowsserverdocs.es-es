---
title: exec
description: Artículo de referencia para el comando exec, que ejecuta un archivo de script en el equipo local.
ms.topic: reference
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0e4cd876fe5c6abcf909f20e330ff347db1113b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023999"
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
