---
title: destino de bdehdcfg
description: 'Tema de comandos de Windows para el destino bdehdcfg: prepara una partición para su uso como unidad del sistema mediante BitLocker y la recuperación de Windows.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2fb0a1daa257ef2c9f1cd77b88e5ef14f84a0dfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382179"
---
# <a name="bdehdcfg-target"></a>bdehdcfg: destino



Prepara una partición para su uso como unidad del sistema mediante BitLocker y la recuperación de Windows. De forma predeterminada, esta partición se crea sin una letra de unidad. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|valor predeterminado|Indica que la herramienta de línea de comandos seguirá el mismo proceso que el asistente para la instalación de BitLocker.|
|unallocated|Crea la partición del sistema a partir del espacio sin asignar disponible en el disco.|
|\<DriveLetter > reducir|Reduce la unidad especificada en la cantidad necesaria para crear una partición activa del sistema. Para utilizar este comando, la unidad especificada debe tener al menos el 5 por ciento de espacio disponible.|
|@no__t 0DriveLetter > Merge|Utiliza la unidad de disco especificada como partición activa del sistema. La unidad del sistema operativo no puede ser un destino para la combinación.|

## <a name="BKMK_Examples"></a>Example

En el siguiente ejemplo se muestra el uso del comando de **destino** para designar una unidad existente (P) como unidad del sistema.
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)