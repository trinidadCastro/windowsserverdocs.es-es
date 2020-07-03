---
title: simulate restore
description: Artículo de referencia para simular la restauración, que comprueba la implicación del escritor en las sesiones de restauración en el equipo sin emitir eventos de restauración o postrestauración a escritores.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cc247848ef4fac1e3a6537247f640f3533c2bcd
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936349"
---
# <a name="simulate-restore"></a>Simular restauración

La implicación del escritor de pruebas en las sesiones de restauración del equipo sin emitir eventos de **restauración** o **postrestauración** a escritores.

## <a name="syntax"></a>Sintaxis

```
simulate restore
```

## <a name="remarks"></a>Comentarios

-   La simulación de la **restauración** se usa para probar si la restauración con escritores puede realizarse correctamente.
-   Antes de poder usar **Simulate restore**, debe cargar un archivo de metadatos de DiskShadow con el comando **cargar metadatos** . Esto carga los escritores y componentes seleccionados para la restauración.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)