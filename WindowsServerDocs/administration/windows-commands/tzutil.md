---
title: tzutil
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41a46ea7974b67cc557973484428480e7beb5484
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876806"
---
# <a name="tzutil"></a>tzutil

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra el Windows tiempo utilidad de zona. 
## <a name="syntax"></a>Sintaxis
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/?|Muestra la ayuda en el símbolo del sistema.|
|/g|Muestra el identificador de zona horaria actual.|
|/s \<timeZoneID > [_dstoff]|Establece la zona horaria actual con el identificador de zona horaria especificada. El **_dstoff** sufijo deshabilita los ajustes del horario de verano de la zona horaria (si procede).|
|/l|listas de hora válidos todos los identificadores de zona y los nombres para mostrar. El resultado será:<br /><br />-   \<nombre para mostrar ><br />-   \<Id. de zona horaria >|

## <a name="remarks"></a>Comentarios
Un código de salida de **0** indica el comando se completó correctamente.

## <a name="BKMK_Examples"></a>Ejemplos
Para mostrar el identificador de zona horaria actual, escriba:
```
tzutil /g
```
Para establecer la zona horaria actual en hora estándar del Pacífico, escriba:
```
tzutil /s Pacific Standard time
```
Para establecer la zona horaria actual a la hora estándar del Pacífico y deshabilita los ajustes del horario de verano, escriba:
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

