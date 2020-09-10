---
title: simulate restore
description: Artículo de referencia para simular la restauración, que comprueba la implicación del escritor en las sesiones de restauración en el equipo sin emitir eventos de restauración o postrestauración a escritores.
ms.topic: reference
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d72e4b473b3913bff744ff7a34b6508bde52ae0e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640932"
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