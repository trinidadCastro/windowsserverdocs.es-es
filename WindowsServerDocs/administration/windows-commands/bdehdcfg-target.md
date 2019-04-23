---
title: destino de bdehdcfg
description: Tema de los comandos de Windows como el destino, bdehdcfg prepara una partición para su uso como una unidad del sistema mediante la recuperación de BitLocker y Windows.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8d180974f480b4c40532dab529ad49dcc33540d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881536"
---
# <a name="bdehdcfg-target"></a>bdehdcfg: target



Prepara una partición para su uso como una unidad del sistema mediante BitLocker y la recuperación de Windows. De forma predeterminada, esta partición se crea sin una letra de unidad. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|valor predeterminado|Indica que la herramienta de línea de comandos seguirá el mismo proceso que el asistente para la instalación de BitLocker.|
|unallocated|Crea la partición del sistema a partir del espacio sin asignar disponible en el disco.|
|\<LetraDeUnidad > reducir|Reduce la unidad especificada en la cantidad necesaria para crear una partición activa del sistema. Para utilizar este comando, la unidad especificada debe tener al menos el 5 por ciento de espacio disponible.|
|\<LetraDeUnidad > fusión mediante combinación|Utiliza la unidad de disco especificada como partición activa del sistema. La unidad del sistema operativo no puede ser un destino para la combinación.|

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente describe el uso de la **destino** comando para designar una unidad existente (P) como la unidad del sistema.
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)