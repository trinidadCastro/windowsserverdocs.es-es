---
title: bdehdcfg quiet
description: Artículo de referencia del comando Quiet de bdehdcfg, que indica a bdehdcfg que no muestre todas las acciones y errores.
ms.topic: reference
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 842788b4413cda828d208e8e7fbbeed28d7a72be
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031533"
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
