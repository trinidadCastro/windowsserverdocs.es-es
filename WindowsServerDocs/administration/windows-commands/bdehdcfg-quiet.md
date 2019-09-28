---
title: bdehdcfg silencioso
description: El tema comandos de Windows para bdehdcfg Quiet-indica a bdehdcfg que no muestre todas las acciones y errores.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d59a14e34200e3fa8e18e36e166ef62ceca1afe7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382220"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg: Quiet



Informa a la herramienta de línea de comandos Bdehdcfg de que no se mostrarán todas las acciones y los errores en la interfaz de la línea de comandos. Para obtener un ejemplo de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Parámetros

Este comando no toma ningún parámetro adicional.

## <a name="remarks"></a>Comentarios

Si se muestran indicaciones Yes/No (Y/N) durante la preparación de la unidad, se supondrá la respuesta "Yes". Para ver los errores que se produzcan durante la preparación de la unidad, revise el registro de eventos del sistema en el proveedor de eventos **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="BKMK_Examples"></a>Example

En el siguiente ejemplo se muestra el uso del comando **Quiet** .
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)