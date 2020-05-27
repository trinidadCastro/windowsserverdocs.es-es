---
title: exit
description: Tema de referencia para Exit, que sale del intérprete de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3cee4a2-6210-46f0-b8e4-7381c3c4e530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdfec9861e63f7484a9c45c45a22d19873cabbe9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819505"
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
| /b | Sale del script por lotes actual en lugar de salir de cmd. exe. Si se ejecuta desde fuera de un script por lotes, sale de cmd. exe. |
| `<exitcode>` | Especifica un número numérico. Si se especifica **/b** , la variable de entorno ERRORLEVEL se establece en ese número. Si sale del intérprete de comandos, el código de salida del proceso se establece en ese número. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para cerrar el intérprete de comandos, escriba:

```
exit
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
