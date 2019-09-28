---
title: active
description: En el tema comandos de Windows para los discos básicos de **activos** , se marca la partición que tiene el foco como activa.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c926bf9b7a583cf7eaa23166e09e6f0a1599e625
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382847"
---
# <a name="active"></a>active



En los discos básicos, marca la partición que tiene el foco como activa.

> [!CAUTION]
> DiskPart solo comprueba que la partición sea capaz de contener los archivos de inicio del sistema operativo. DiskPart no comprueba el contenido de la partición. Si marca una partición como activa erróneamente y no contiene los archivos de inicio del sistema operativo, es posible que el equipo no se inicie.

## <a name="syntax"></a>Sintaxis

```
active
```

## <a name="remarks"></a>Comentarios

-   Esto informa al sistema básico de entrada y salida (BIOS) o Extensible Firmware Interface (EFI) de que la partición o el volumen es una partición del sistema o un volumen del sistema válidos.
-   Solo se pueden marcar como activas las particiones.
-   Se debe seleccionar una partición para que esta operación se realice correctamente. Use el comando **seleccionar partición** para seleccionar una partición y desplazar el foco a ella.

## <a name="BKMK_examples"></a>Example

Para marcar la partición que tiene el foco como la partición activa, escriba:
```
active
```

#### <a name="additional-references"></a>Referencias adicionales

