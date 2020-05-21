---
title: expand
description: Tema de referencia del comando Expand, que expande uno o más archivos comprimidos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a25507b83c17100c579f00d10c94e20c6be2aa4e
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437220"
---
# <a name="expand"></a>expand

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Expande uno o más archivos comprimidos. También puede usar este comando para recuperar archivos comprimidos de discos de distribución.

El comando **Expand** también se puede ejecutar desde la consola de recuperación de Windows, con parámetros diferentes. Para obtener más información, vea [entorno de recuperación de Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxis

```
expand [/r] <source> <destination>
expand /r <source> [<destination>]
expand /i <source> [<destination>]
expand /d <source>.cab [/f:<files>]
expand <source>.cab /f:<files> <destination>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /r | Cambia el nombre de los archivos expandidos. |
| origen | Especifica los archivos que se van a expandir. El *origen* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos. Puede usar caracteres comodín (**&#42;** o **?**). |
| destination | Especifica la ubicación donde se van a expandir los archivos.<p>Si el *origen* consta de varios archivos y no especifica **/r**, el *destino* debe ser un directorio. El *destino* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos. Especificación de destino `file | path` . |
| /i | Cambia el nombre de los archivos expandidos pero omite la estructura de directorios. |
| /d | Muestra una lista de los archivos que se encuentran en el origen. No expande ni extrae los archivos. |
| /f`<files>` | Especifica los archivos de un archivo contenedor (. cab) que desea expandir. Puede usar caracteres comodín (**&#42;** o **?**). |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

