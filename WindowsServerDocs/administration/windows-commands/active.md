---
title: active
description: Tema de los comandos de Windows para **active** - en discos básicos, marca la partición con el foco como activa.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3a039e0200fb84d446739ac7017556b6c302f4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868766"
---
# <a name="active"></a>active



En discos básicos, marca la partición tiene el foco como activa.

> [!CAUTION]
> DiskPart sólo comprueba que la partición es capaz de contener los archivos de inicio del sistema operativo. DiskPart no comprueba el contenido de la partición. Si se marca una partición como activa por error y no contiene los archivos de inicio del sistema operativo, el equipo podría no iniciarse.

## <a name="syntax"></a>Sintaxis

```
active
```

## <a name="remarks"></a>Comentarios

-   Esto se informa al sistema básico de entrada/salida (BIOS) o Extensible Firmware Interface (EFI) de que la partición o volumen es una partición de sistema válido o el volumen del sistema.
-   Solo las particiones se pueden marcar como activas.
-   Debe seleccionarse una partición para que esta operación se realice correctamente. Use la **seleccione partición** comando para seleccionar una partición y desplace el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para marcar la partición tiene el foco como la partición activa, escriba:
```
active
```

#### <a name="additional-references"></a>Referencias adicionales

