---
title: recover
description: Artículo de referencia del comando Recover, que recupera información legible de un disco defectuoso o defectuoso.
ms.topic: reference
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9bb402c5821ae8caa0a83d132a7fbeef9c88b4bd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637308"
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
