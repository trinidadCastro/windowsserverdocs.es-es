---
title: recover
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 172471c5c56823e29dd0882072920db3d77639da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836728"
---
# <a name="recover"></a>recover



Recupera información legible de un disco defectuoso o defectuoso.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
recover [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parámetros

|           Parámetro           |                                          Descripción                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<> de unidad:] [<Path>]<FileName> | Especifica la ubicación y el nombre del archivo que desea recuperar. *Filename* es obligatorio. |
|              /?               |                             Muestra la Ayuda en el símbolo del sistema.                              |

## <a name="remarks"></a>Comentarios

-   El comando **Recover** Lee un archivo, sector por sector, y recupera los datos de los sectores correctos. Se pierden los datos de los sectores dañados.
-   Los sectores dañados indicados por **CHKDSK** se marcaron como no válidos cuando el disco estaba preparado para su funcionamiento. No suponen ningún riesgo y la **recuperación** no los afecta.
-   Dado que todos los datos de los sectores dañados se pierden al recuperar un archivo, solo debe recuperar un archivo cada vez.
-   No se pueden usar caracteres comodín **&#42;** (y **?** ) con el comando **Recover** . Debe especificar un archivo (y la ubicación del archivo si no está en el directorio actual).

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para recuperar el archivo Story. txt en el directorio \Fiction de la unidad D, escriba:
```
recover d:\fiction\story.txt 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)