---
title: bdehdcfg restart
description: Artículo de referencia para el comando bdehdcfg restart, que indica a bdehdcfg que el equipo debe reiniciarse una vez finalizada la preparación de la unidad.
ms.topic: reference
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8db8ede29d5cecfdbe29031c21f86c5e2b8fc8c1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031493"
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
