---
title: reinicio de bdehdcfg
description: Tema de los comandos de Windows para el reinicio de bdehdcfg - indica bdehdcfg que se debe reiniciar el equipo una vez concluida la preparación de la unidad.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f361db8fdf33bd414556575de75241f7dbd9327
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879466"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg: restart



Informa a la herramienta de línea de comandos de Bdehdcfg que se debe reiniciar el equipo una vez concluida la preparación de la unidad. Para obtener un ejemplo de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Parámetros

Este comando toma ningún parámetro adicional.

## <a name="remarks"></a>Comentarios

Si otros usuarios se registran el equipo y el **silencioso** comando no se especifica, se mostrará un símbolo del sistema para confirmar que se debe reiniciar el equipo.

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente se muestra cómo utilizar el **reiniciar** comando.
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)