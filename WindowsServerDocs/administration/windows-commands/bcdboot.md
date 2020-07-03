---
title: bcdboot
description: Artículo de referencia para el comando de BCDboot, que configura rápidamente una partición del sistema o repara el entorno de arranque ubicado en la partición del sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: def4052e8aaa4f1e32216b5de837706b5cde3d04
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923510"
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

Para obtener información sobre dónde encontrar BCDboot y ejemplos de cómo usar este comando, consulte el tema [Opciones de la línea de comandos de BCDboot](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)) .

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
