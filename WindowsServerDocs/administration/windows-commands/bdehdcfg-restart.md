---
title: bdehdcfg restart
description: Artículo de referencia para el comando bdehdcfg restart, que indica a bdehdcfg que el equipo debe reiniciarse una vez finalizada la preparación de la unidad.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02d57eaef9b2f6e705a5bb30d7d52a09579d27c9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923460"
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
