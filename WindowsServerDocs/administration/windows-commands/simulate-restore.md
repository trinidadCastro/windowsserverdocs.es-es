---
title: Simular la restauración
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 09b626939c13d4e38a983435b45d8c47ee2b93a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817256"
---
# <a name="simulate-restore"></a>Simular la restauración



Comprueba la participación del sistema de escritura en las sesiones de restauración en el equipo sin emitir **PreRestore** o **PostRestore** eventos a los escritores.

## <a name="syntax"></a>Sintaxis

```
simulate restore
```

## <a name="remarks"></a>Comentarios

-   **Simular la restauración** se usa para probar si la restauración con escritores puede tener éxito.
-   Para poder usar **simular restauración**, debe cargar un archivo de metadatos de DiskShadow mediante el **cargar metadatos** comando. Esto carga los escritores seleccionados y los componentes para la restauración.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)