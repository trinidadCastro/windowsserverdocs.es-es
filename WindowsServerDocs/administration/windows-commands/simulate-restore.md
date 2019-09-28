---
title: Simular restauración
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6652fea4e74c706fcc03b8a547fab771a7c0191
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370885"
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

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)