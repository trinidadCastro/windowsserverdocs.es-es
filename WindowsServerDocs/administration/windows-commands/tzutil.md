---
title: tzutil
description: Artículo de referencia de tzutil, que muestra la utilidad de zona horaria de Windows.
ms.topic: reference
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d8eae64faf58d404c49afa5b469c61d44807ae0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029943"
---
# <a name="tzutil"></a>tzutil

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra la utilidad de zona horaria de Windows.

## <a name="syntax"></a>Sintaxis
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/?|Muestra la ayuda en el símbolo del sistema.|
|/g|Muestra el ID. de zona horaria actual.|
|/s \<timeZoneID> [_dstoff]|Establece la zona horaria actual mediante el identificador de zona horaria especificado. El sufijo **_dstoff** deshabilita los ajustes del horario de verano de la zona horaria (si procede).|
|/l|enumera todos los ID. de zona horaria y nombres para mostrar válidos. El resultado será:<p>-   \<display name><br />-   \<time zone ID>|

## <a name="remarks"></a>Observaciones
Un código de salida de **0** indica que el comando se completó correctamente.

## <a name="examples"></a>Ejemplos
Para mostrar el ID. de zona horaria actual, escriba:
```
tzutil /g
```
Para establecer la zona horaria actual en hora estándar del Pacífico, escriba:
```
tzutil /s Pacific Standard time
```
Para establecer la zona horaria actual en hora estándar del Pacífico y deshabilitar los ajustes del horario de verano, escriba:
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

