---
title: bdehdcfg no interactivo
description: Tema de los comandos de Windows para bdehdcfg quiet - indica bdehdcfg que no se mostrarán todas las acciones y errores.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f0d98f6ae76e9bf6357689c97e091766b9645c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865756"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg: quiet



Informa a la herramienta de línea de comandos de Bdehdcfg que todas las acciones y los errores no se muestra en la interfaz de línea de comandos. Para obtener un ejemplo de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Parámetros

Este comando toma ningún parámetro adicional.

## <a name="remarks"></a>Comentarios

Si se muestran indicaciones Yes/No (Y/N) durante la preparación de la unidad, se supondrá la respuesta "Yes". Para ver los errores que se produzcan durante la preparación de la unidad, revise el registro de eventos del sistema en el proveedor de eventos **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente se muestra cómo utilizar el **silencioso** comando.
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)