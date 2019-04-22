---
title: sxstrace
description: Obtenga información sobre cómo diagnosticar problemas en paralelo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 396d06bf079c0cfa8ba4864f71333eec39f7b255
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814106"
---
# <a name="sxstrace"></a>sxstrace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diagnostica problemas en paralelo.    

## <a name="syntax"></a>Sintaxis  
```  
sxstrace [{[trace /logfile:<FileName> [/nostop]|[parse /logfile:<FileName> /outfile:<ParsedFile>  [/filter:<AppName>]}]  
```  

### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|seguimiento|Habilita el seguimiento de sxs (side-by-side)|  
|/logfile|Especifica el archivo de registro sin procesar.|  
|\<FileName>|Guarda el registro de seguimiento *FileName*.|  
|/nostop|Especifica que no hay símbolo para detener el seguimiento.|  
|Analizar|Convierte el archivo de seguimiento sin procesar.|  
|/ outfile|Especifica el nombre de archivo de salida.|  
|\<ParsedFile>|Especifica el nombre de archivo del archivo analizado.|  
|/filter|Permite filtrar el resultado.|  
|\<AppName>|Especifica el nombre de la aplicación.|  
|stoptrace|Si no se detiene antes, detenga el seguimiento.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="BKMK_Examples"></a>Ejemplos  
Habilitar el seguimiento y guardar el archivo de seguimiento **sxstrace.etl**:  
```  
sxstrace trace /logfile:sxstrace.etl  
```  
Traducir el archivo de seguimiento sin procesar en un formato legible y guardar el resultado a **sxstrace.txt**:  
```  
sxstrace parse /logfile:sxstrace.etl /outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
