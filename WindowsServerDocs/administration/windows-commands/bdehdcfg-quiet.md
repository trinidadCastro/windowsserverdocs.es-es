---
title: bdehdcfg quiet
description: Artículo de referencia del comando Quiet de bdehdcfg, que indica a bdehdcfg que no muestre todas las acciones y errores.
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ddd6e2cb63b6af0ea0c50b5260436c184ee6aa6
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895085"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg: Quiet

Informa a la herramienta de línea de comandos bdehdcfg de que no se mostrarán todas las acciones y los errores en la interfaz de la línea de comandos. Los mensajes sí/no (Y/N) que se muestran durante la preparación de la unidad asumirán una respuesta "sí". Para ver los errores que se produzcan durante la preparación de la unidad, revise el registro de eventos del sistema en el proveedor de eventos **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -quiet
```

#### <a name="parameters"></a>Parámetros

Este comando no tiene parámetros adicionales.

## <a name="examples"></a>Ejemplos

Para usar el comando **Quiet** :

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
