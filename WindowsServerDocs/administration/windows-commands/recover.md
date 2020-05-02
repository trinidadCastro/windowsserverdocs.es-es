---
title: recover
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b316ed26f008a62f88aaeb4a7a7f3030d08f1588
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722622"
---
# <a name="recover"></a>recover



Recupera información legible de un disco defectuoso o defectuoso.



## <a name="syntax"></a>Sintaxis

```
recover [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parámetros

|           Parámetro           |                                          Descripción                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<> de unidad:] [<Path>]<FileName> | Especifica la ubicación y el nombre del archivo que desea recuperar. *Filename* es obligatorio. |
|              /?               |                             Muestra la ayuda en el símbolo del sistema.                              |

## <a name="remarks"></a>Observaciones

-   El comando **Recover** Lee un archivo, sector por sector, y recupera los datos de los sectores correctos. Se pierden los datos de los sectores dañados.
-   Los sectores dañados indicados por **CHKDSK** se marcaron como no válidos cuando el disco estaba preparado para su funcionamiento. No suponen ningún riesgo y la **recuperación** no los afecta.
-   Dado que todos los datos de los sectores dañados se pierden al recuperar un archivo, solo debe recuperar un archivo cada vez.
-   No se pueden usar caracteres comodín (**&#42;** y **?**) con el comando **Recover** . Debe especificar un archivo (y la ubicación del archivo si no está en el directorio actual).

## <a name="examples"></a>Ejemplos

Para recuperar el archivo Story. txt en el directorio \Fiction de la unidad D, escriba:
```
recover d:\fiction\story.txt 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)