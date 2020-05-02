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
ms.openlocfilehash: 212e2d45b77f09b9460555733de15488a4420842
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721596"
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
|\<Nombre de archivo>|Guarda el registro de seguimiento en el *nombre de archivo*.|  
|-NoStop|No especifica ningún aviso para detener el seguimiento.|  
|parse|Traduce el archivo de seguimiento sin procesar.|  
|-OUTFILE|Especifica el nombre de archivo de salida.|  
|\<> ParsedFile|Especifica el nombre de archivo del archivo analizado.|  
|-filter|Permite filtrar la salida.|  
|\<AppName>|Especifica el nombre de la aplicación.|  
|stoptrace|Detenga el seguimiento si no se ha detenido antes de.|  
|-?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="examples"></a>Ejemplos  
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
  
