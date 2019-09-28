---
title: tamaño de bdehdcfg
description: 'Temas de comandos de Windows: especifica el tamaño de la partición del sistema cuando se crea una nueva unidad del sistema.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ec42cdb5716c63c7210ea6cfde8ce8884833b45
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382200"
---
# <a name="bdehdcfg-size"></a>bdehdcfg: tamaño



Especifica el tamaño de la partición del sistema cuando se crea una nueva unidad del sistema. Para obtener un ejemplo de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0SizeinMB >|Indica el número de megabytes (MB) que se va a utilizar para la nueva partición.|

## <a name="remarks"></a>Comentarios

Si no especifica un tamaño, la herramienta utilizará el valor predeterminado de 300 MB. El tamaño mínimo de la unidad del sistema es 100 MB. Si va a almacenar las herramientas de recuperación del sistema u otras herramientas del sistema en la partición del sistema, deberá aumentar el tamaño consiguientemente.

> [!NOTE]
> El comando **size** no se puede combinar con el comando **target** \<DriveLetter > **Merge** .

## <a name="BKMK_Examples"></a>Example

En el siguiente ejemplo se muestra el uso del comando **size** para asignar 500 MB a la unidad del sistema predeterminada.
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)