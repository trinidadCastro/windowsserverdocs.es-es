---
title: exit
description: Artículo de referencia para Exit, que sale del intérprete de comandos.
ms.topic: article
ms.assetid: d3cee4a2-6210-46f0-b8e4-7381c3c4e530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 058e41d49ece470421fbd2b160037885b92c1bed
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890469"
---
# <a name="exit"></a>exit

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Sale del intérprete de comandos o del script por lotes actual.

## <a name="syntax"></a>Sintaxis

```
exit [/b] [<exitcode>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /b | Sale del script por lotes actual en lugar de salir de Cmd.exe. Si se ejecuta desde fuera de un script por lotes, sale Cmd.exe. |
| `<exitcode>` | Especifica un número numérico. Si se especifica **/b** , la variable de entorno ERRORLEVEL se establece en ese número. Si sale del intérprete de comandos, el código de salida del proceso se establece en ese número. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para cerrar el intérprete de comandos, escriba:

```
exit
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
