---
title: tzutil
description: Artículo de referencia de tzutil, que muestra la utilidad de zona horaria de Windows.
ms.topic: reference
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3640da68f48944fd9d67486dface4cfd77531d57
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078462"
---
# <a name="tzutil"></a>tzutil

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra la utilidad de zona horaria de Windows.

## <a name="syntax"></a>Sintaxis

```
tzutil [/?] [/g] [/s <timezoneID>[_dstoff]] [/l]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /g | Muestra el ID. de zona horaria actual. |
| modificado `<timezoneID>[_dstoff]` | Establece la zona horaria actual mediante el identificador de zona horaria especificado. El sufijo **_dstoff** deshabilita los ajustes del horario de verano de la zona horaria (si procede). El valor debe ir entre comillas. |
| /l | Enumera todos los ID. de zona horaria y nombres para mostrar válidos. La salida aparece como:<ul><li>`<display name>`</li><li>`<time zone ID>`</li></ul> |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

Un código de salida de **0** indica que el comando se completó correctamente.

## <a name="examples"></a>Ejemplos

Para mostrar el ID. de zona horaria actual, escriba:

```
tzutil /g
```

Para establecer la zona horaria actual en hora estándar del Pacífico, escriba:

```
tzutil /s "Pacific Standard time"
```

Para establecer la zona horaria actual en hora estándar del Pacífico y deshabilitar los ajustes del horario de verano, escriba:

```
tzutil /s "Pacific Standard time_dstoff"
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
