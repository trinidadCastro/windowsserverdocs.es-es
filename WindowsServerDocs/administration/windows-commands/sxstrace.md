---
title: sxstrace
description: Obtenga información sobre cómo diagnosticar problemas en paralelo.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 66326943bf1b056951ae5824df5a4f60892492cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370711"
---
# <a name="sxstrace"></a>sxstrace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diagnostica problemas en paralelo.    

## <a name="syntax"></a>Sintaxis  
```  
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]  
```  

### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|Seguimiento|Habilita el seguimiento para SxS (en paralelo)|  
|-logfile|Especifica el archivo de registro sin procesar.|  
|\<Nombre de archivo >|Guarda el registro de seguimiento en el *nombre de archivo*.|  
|-NoStop|No especifica ningún aviso para detener el seguimiento.|  
|Analiza|Traduce el archivo de seguimiento sin procesar.|  
|-OUTFILE|Especifica el nombre de archivo de salida.|  
|\<> ParsedFile|Especifica el nombre de archivo del archivo analizado.|  
|-filtro|Permite filtrar la salida.|  
|\<AppName >|Especifica el nombre de la aplicación.|  
|stoptrace|Detenga el seguimiento si no se ha detenido antes de.|  
|-?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="BKMK_Examples"></a>Example  
Habilite el seguimiento y guarde el archivo de seguimiento en **sxstrace. ETL**:  
```  
sxstrace trace -logfile:sxstrace.etl  
```  
Traduzca el archivo de seguimiento sin procesar en un formato legible para el usuario y guarde el resultado en **sxstrace. txt**:  
```  
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
