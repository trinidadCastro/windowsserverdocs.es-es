---
title: simulate restore
description: Artículo de referencia para simular la restauración, que comprueba la implicación del escritor en las sesiones de restauración en el equipo sin emitir eventos de restauración o postrestauración a escritores.
ms.topic: reference
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5640f69f421d65588251ff1e15b63cbeaffde613
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036983"
---
# <a name="simulate-restore"></a>Simular restauración

La implicación del escritor de pruebas en las sesiones de restauración del equipo sin emitir eventos de **restauración** o **postrestauración** a escritores.

## <a name="syntax"></a>Sintaxis

```
simulate restore
```

## <a name="remarks"></a>Observaciones

-   La simulación de la **restauración** se usa para probar si la restauración con escritores puede realizarse correctamente.
-   Antes de poder usar **Simulate restore**, debe cargar un archivo de metadatos de DiskShadow con el comando **cargar metadatos** . Esto carga los escritores y componentes seleccionados para la restauración.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)