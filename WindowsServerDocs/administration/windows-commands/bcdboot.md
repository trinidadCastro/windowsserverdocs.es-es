---
title: bcdboot
description: Windows Commands topic for **BCDboot**, que configuran rápidamente una partición del sistema, o reparan el entorno de arranque ubicado en la partición del sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 637170cbd8ee4f3c11ee1dc77a0cd49b5dfa3038
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851088"
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
| fuentes | Especifica la ubicación del directorio de Windows que se va a usar como origen para copiar los archivos del entorno de arranque. |
| /l | Especifica la configuración regional. La configuración regional predeterminada es Inglés de EE. UU. |
| /s | Especifica la letra de volumen de la partición del sistema. El valor predeterminado es la partición del sistema identificada por el firmware. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para obtener información sobre dónde encontrar BCDboot y ejemplos de cómo usar este comando, consulte el tema [Opciones de la línea de comandos de BCDboot](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)x) .

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)