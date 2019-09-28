---
title: recover
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 415efe2d1e60ca70d68059b5702108440da735f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371772"
---
# <a name="recover"></a>recover



Recupera información legible de un disco defectuoso o defectuoso.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parámetros

|           Parámetro           |                                          Descripción                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Drive >:] [<Path>] <FileName> | Especifica la ubicación y el nombre del archivo que desea recuperar. *Filename* es obligatorio. |
|              /?               |                             Muestra la ayuda en el símbolo del sistema.                              |

## <a name="remarks"></a>Comentarios

-   El comando **Recover** Lee un archivo, sector por sector, y recupera los datos de los sectores correctos. Se pierden los datos de los sectores dañados.
-   Los sectores dañados indicados por **CHKDSK** se marcaron como "incorrectos" cuando el disco estaba preparado para su funcionamiento. No suponen ningún riesgo y la **recuperación** no los afecta.
-   Dado que todos los datos de los sectores dañados se pierden al recuperar un archivo, solo debe recuperar un archivo cada vez.
-   No se pueden usar caracteres comodín **&#42;** (y **?** ) con el comando **Recover** . Debe especificar un archivo (y la ubicación del archivo si no está en el directorio actual).

## <a name="BKMK_examples"></a>Example

Para recuperar el archivo Story. txt en el directorio \Fiction de la unidad D, escriba:
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)