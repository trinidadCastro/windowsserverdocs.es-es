---
title: activo
description: El tema comandos de Windows para **Active**, que en discos básicos, marca la partición que tiene el foco como activa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42f2e0d367344355e8f9a570f37cfbdc5dfc4590
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851378"
---
# <a name="active"></a>activo

En discos básicos, marca como activa la partición que tiene el foco.

> [!CAUTION]
> DiskPart solo comprueba que la partición sea capaz de contener los archivos de inicio del sistema operativo. DiskPart no comprueba el contenido de la partición. Si marca una partición como activa erróneamente y no contiene los archivos de inicio del sistema operativo, es posible que el equipo no se inicie.

## <a name="syntax"></a>Sintaxis

```
active
```- 

## Remarks

-   This informs the basic input/output system (BIOS) or Extensible Firmware Interface (EFI) that the partition or volume is a valid system partition or system volume.

-   Only partitions can be marked as active.

-   A partition must be selected for this operation to succeed. Use the **select partition** command to select a partition and shift the focus to it.

## <a name=BKMK_examples></a>Examples

To mark the partition with focus as the active partition, type:

```
activo
```
## Additional References

- [Command-Line Syntax Key](command-line-syntax-key.md)