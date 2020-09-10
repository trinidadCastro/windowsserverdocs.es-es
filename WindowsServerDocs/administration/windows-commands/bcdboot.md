---
title: bcdboot
description: Artículo de referencia para el comando de BCDboot, que configura rápidamente una partición del sistema o repara el entorno de arranque ubicado en la partición del sistema.
ms.topic: reference
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b7cc67c47093686882f0a7bfa1fcc47f8c444210
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632980"
---
# <a name="bcdboot"></a>bcdboot

Permite configurar rápidamente una partición del sistema o reparar el entorno de arranque ubicado en la partición del sistema. La partición del sistema se configura mediante la copia de un conjunto simple de archivos datos de la configuración de arranque (BCD) (BCD) en una partición vacía existente.

## <a name="syntax"></a>Sintaxis

```
bcdboot <source> [/l] [/s]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| source | Especifica la ubicación del directorio de Windows que se va a usar como origen para copiar los archivos del entorno de arranque. |
| /l | Especifica la configuración regional. La configuración regional predeterminada es Inglés de EE. UU. |
| /s | Especifica la letra de volumen de la partición del sistema. El valor predeterminado es la partición del sistema identificada por el firmware. |

## <a name="examples"></a>Ejemplos

Para obtener información sobre dónde encontrar BCDboot y ejemplos de cómo usar este comando, consulte el tema [Opciones de la línea de comandos de BCDboot](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)) .

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
