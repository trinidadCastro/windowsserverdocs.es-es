---
title: bdehdcfg restart
description: Artículo de referencia para el comando bdehdcfg restart, que indica a bdehdcfg que el equipo debe reiniciarse una vez finalizada la preparación de la unidad.
ms.topic: reference
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0839f22bed8dee63fb25f9b254e36b0c02c88399
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632836"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg: reinicio

Informa a la herramienta de línea de comandos bdehdcfg de que el equipo debe reiniciarse una vez finalizada la preparación de la unidad. Si otros usuarios han iniciado sesión en el equipo y no se ha especificado el comando **Quiet** , aparecerá un mensaje para confirmar que el equipo debe reiniciarse.

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -restart
```

#### <a name="parameters"></a>Parámetros

Este comando no tiene parámetros adicionales.

## <a name="examples"></a>Ejemplos

Para usar el comando de **reinicio** :

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
