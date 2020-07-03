---
title: mklink
description: Artículo de referencia para el comando Mklink, que crea un directorio o un vínculo simbólico o de archivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb8339f7dcb2f397d6b90105e2ccd9bdc8cc07a5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928532"
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

Para crear y quitar un vínculo simbólico denominado, carpeta y archivo. File, desde el directorio raíz al directorio \Users\User1\Documents y un archivo example ubicado en el directorio, escriba:

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

- [New-item en Windows PowerShell](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
