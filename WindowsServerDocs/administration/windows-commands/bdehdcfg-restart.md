---
title: reinicio de bdehdcfg
description: 'Windows Commands topic for bdehdcfg restart: indica a bdehdcfg que el equipo debe reiniciarse una vez finalizada la preparación de la unidad.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6c4e48b051f567c98ea679feaa22f995982a899
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382215"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg: reinicio



Informa a la herramienta de línea de comandos Bdehdcfg de que el equipo debe reiniciarse una vez finalizada la preparación de la unidad. Para obtener un ejemplo de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Parámetros

Este comando no toma ningún parámetro adicional.

## <a name="remarks"></a>Comentarios

Si otros usuarios han iniciado sesión en el equipo y no se ha especificado el comando **Quiet** , se mostrará un símbolo del sistema para confirmar que el equipo debe reiniciarse.

## <a name="BKMK_Examples"></a>Example

En el siguiente ejemplo se muestra el uso del comando **restart** .
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)