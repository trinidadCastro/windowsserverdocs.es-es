---
title: sxstrace
description: Obtenga información sobre cómo diagnosticar problemas en paralelo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ece727b68eb620e839cbfb8efe02dbe775666498
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833618"
---
# <a name="sxstrace"></a>sxstrace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|\<nombre de archivo >|Guarda el registro de seguimiento en el *nombre de archivo*.|  
|-NoStop|No especifica ningún aviso para detener el seguimiento.|  
|analiza|Traduce el archivo de seguimiento sin procesar.|  
|-OUTFILE|Especifica el nombre de archivo de salida.|  
|\<ParsedFile >|Especifica el nombre de archivo del archivo analizado.|  
|-filtro|Permite filtrar la salida.|  
|\<AppName >|Especifica el nombre de la aplicación.|  
|stoptrace|Detenga el seguimiento si no se ha detenido antes de.|  
|-?|Muestra la Ayuda en el símbolo del sistema.|  

## <a name="examples"></a><a name="BKMK_Examples"></a>Example  
Habilite el seguimiento y guarde el archivo de seguimiento en **sxstrace. ETL**:  
```  
sxstrace trace -logfile:sxstrace.etl  
```  
Traduzca el archivo de seguimiento sin procesar en un formato legible para el usuario y guarde el resultado en **sxstrace. txt**:  
```  
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
