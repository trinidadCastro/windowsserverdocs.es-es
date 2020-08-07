---
title: sxstrace
description: Obtenga información sobre cómo diagnosticar problemas en paralelo.
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1ed136e72569c2dfbe59cd2132e13c23f94da02
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881938"
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

