---
title: bdehdcfg restart
description: Artículo de referencia para el comando bdehdcfg restart, que indica a bdehdcfg que el equipo debe reiniciarse una vez finalizada la preparación de la unidad.
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67d2a3dd7b4304c26543840d6681b4ec6e655651
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895095"
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
