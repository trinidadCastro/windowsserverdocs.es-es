---
title: lpq
description: Tema de referencia del comando lpq, que muestra el estado de una cola de impresión en un equipo que ejecuta line Printer daemon (LPD).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ecce9b1b4255e5e769fe76b0f753226d61fa916
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222725"
---
# <a name="lpq"></a>lpq

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra el estado de una cola de impresión en un equipo que ejecuta line Printer daemon (LPD).

## <a name="syntax"></a>Sintaxis

```
lpq -S <servername> -P <printername> [-l]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -S`<servername>` | Especifica (por nombre o dirección IP) el equipo o el dispositivo de uso compartido de impresoras que hospeda la cola de impresión de LPD con el estado que desea mostrar. Este parámetro es obligatorio y debe escribirse en mayúsculas. |
| -P`<Printername>` | Especifica (por nombre) la impresora para la cola de impresión con el estado que desea mostrar. Este parámetro es obligatorio y debe escribirse en mayúsculas. |
| -l | Especifica que desea mostrar detalles sobre el estado de la cola de impresión. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para mostrar el estado de la cola de impresión de *Laserprinter1* en un host de LPD en *10.0.0.45*, escriba:

```
lpq -S 10.0.0.45 -P Laserprinter1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
