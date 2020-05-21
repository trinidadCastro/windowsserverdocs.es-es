---
title: exec
description: Tema de referencia para el comando exec, que ejecuta un archivo de script en el equipo local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 956f3d4a7c5992980aea0fc0f5933ee7def48381
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436111"
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
