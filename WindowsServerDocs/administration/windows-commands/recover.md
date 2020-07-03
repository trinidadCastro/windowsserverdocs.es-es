---
title: recover
description: Artículo de referencia del comando Recover, que recupera información legible de un disco defectuoso o defectuoso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7f502b046bf30a40b1fdd386c7faddc5c8f15a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931936"
---
# <a name="recover"></a>recover

Recupera información legible de un disco defectuoso o defectuoso. Este comando Lee un archivo, sector por sector, y recupera los datos de los sectores correctos. Se pierden los datos de los sectores dañados. Dado que todos los datos de los sectores dañados se pierden al recuperar un archivo, solo debe recuperar un archivo cada vez.

Los sectores dañados indicados por el comando **CHKDSK** se marcaron como no válidos cuando el disco estaba preparado para su funcionamiento. No suponen ningún riesgo y la **recuperación** no los afecta.

## <a name="syntax"></a>Sintaxis

```
recover [<drive>:][<path>]<filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[<drive>:][<path>]<filename>` | Especifica el nombre de archivo (y la ubicación del archivo si no está en el directorio actual) que desea recuperar. El *nombre de archivo* es obligatorio y no se admiten los caracteres comodín. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para recuperar el archivo *story.txt* en el directorio *\fiction* de la unidad D, escriba:

```
recover d:\fiction\story.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
