---
title: sxstrace
description: Obtenga información sobre cómo diagnosticar problemas en paralelo.
ms.topic: reference
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7afcb79bf12cf23a66ac564362012c4424e5bbe6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626773"
---
# <a name="sxstrace"></a>sxstrace

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Diagnostica problemas en paralelo.

## <a name="syntax"></a>Sintaxis
```
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]
```

#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|seguimiento|Habilita el seguimiento para SxS (en paralelo)|
|-logfile|Especifica el archivo de registro sin procesar.|
|\<FileName>|Guarda el registro de seguimiento en el *nombre de archivo*.|
|-NoStop|No especifica ningún aviso para detener el seguimiento.|
|parse|Traduce el archivo de seguimiento sin procesar.|
|-OUTFILE|Especifica el nombre de archivo de salida.|
|\<ParsedFile>|Especifica el nombre de archivo del archivo analizado.|
|-filter|Permite filtrar la salida.|
|\<AppName>|Especifica el nombre de la aplicación.|
|stoptrace|Detenga el seguimiento si no se ha detenido antes de.|
|-?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos
Habilite el seguimiento y guarde el archivo de seguimiento en **sxstrace. ETL**:
```
sxstrace trace -logfile:sxstrace.etl
```
Traduzca el archivo de seguimiento sin procesar en un formato legible y guarde el resultado en **sxstrace.txt**:
```
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

