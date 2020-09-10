---
title: lpq
description: Artículo de referencia para el comando lpq, que muestra el estado de una cola de impresión en un equipo que ejecuta line Printer daemon (LPD).
ms.topic: reference
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f112216f2692f1bef5156566c39431f4ecebc7a8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636578"
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
| -S `<servername>` | Especifica (por nombre o dirección IP) el equipo o el dispositivo de uso compartido de impresoras que hospeda la cola de impresión de LPD con el estado que desea mostrar. Este parámetro es obligatorio y debe escribirse en mayúsculas. |
| -P `<Printername>` | Especifica (por nombre) la impresora para la cola de impresión con el estado que desea mostrar. Este parámetro es obligatorio y debe escribirse en mayúsculas. |
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
