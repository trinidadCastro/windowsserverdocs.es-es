---
title: Simular restauración
description: Temas de comandos de Windows para simular restauración, que prueba la implicación del escritor en las sesiones de restauración en el equipo sin emitir eventos de restauración o postrestauración a escritores.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 024d654864c000e44bccb9ddb167c6147444cc00
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834108"
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