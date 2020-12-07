---
title: mklink
description: Artículo de referencia para el comando Mklink, que crea un directorio o un vínculo simbólico o de archivo.
ms.topic: reference
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dd605283396757f1ef0de05d620583c895f71f1a
ms.sourcegitcommit: f18097c21e50a09aef2f1937f52608b0042ef0e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96755312"
---
# <a name="mklink"></a>mklink

Crea un directorio o un vínculo simbólico o de archivo.

## <a name="syntax"></a>Sintaxis

```
mklink [[/d] | [/h] | [/j]] <link> <target>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /d | Crea un vínculo simbólico de directorio. De forma predeterminada, este comando crea un vínculo simbólico de archivo. |
| /h | Crea un vínculo físico en lugar de un vínculo simbólico. |
| /j | Crea una Unión de directorio. |
| `<link>` | Especifica el nombre del vínculo simbólico que se va a crear. |
| `<target>` | Especifica la ruta de acceso (relativa o absoluta) a la que hace referencia el nuevo vínculo simbólico. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para crear y quitar un vínculo simbólico denominado mi carpeta desde el directorio raíz al directorio \Users\User1\Documents y un vínculo físico denominado archivo. archivo al archivo example. File ubicado en el directorio, escriba:

```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [del comando](del.md)

- [Rd (comando)](rd.md)

- [New-item en Windows PowerShell](/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
