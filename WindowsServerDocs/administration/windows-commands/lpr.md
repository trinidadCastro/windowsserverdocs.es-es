---
title: lpr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e21b1606b09c47ea773c4fad80eaa53d9aca27c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888756"
---
# <a name="lpr"></a>lpr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envía un archivo a un equipo o dispositivo que ejecuta el servicio Line printer Daemon (LPD) como preparación para la impresión de compartir impresoras.  
  
## <a name="syntax"></a>Sintaxis  
```  
lpr [-S <ServerName>] -P <printerName> [-C <BannerContent>] [-J <JobName>] [-o | "-o l"] [-x] [-d] <filename>  
```  
## <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|-S <ServerName>|Especifica (por nombre o dirección IP), el equipo o dispositivo que hospeda la cola de impresión de LPD con un estado que desea mostrar de compartir impresoras. Obligatorio.|  
|-P <printerName>|Especifica (por nombre) la impresora para la cola de impresión con un estado que desea mostrar. Obligatorio.|  
|-C <BannerContent>|Especifica el contenido para imprimir en la página de encabezado del trabajo de impresión. Si no incluye este parámetro, el nombre del equipo desde el que se envió el trabajo de impresión aparece en la página de encabezado.|  
|-J <JobName>|Especifica el nombre del trabajo de impresión que se imprimirán en la página de encabezado. Si no incluye este parámetro, el nombre del archivo que se va a imprimir aparece en la página de encabezado.|  
|[-o&#124; "-o l"]|Especifica el tipo de archivo que desea imprimir. El parámetro **-o** especifica que desea imprimir un archivo de texto. El parámetro **"-o l"** especifica que desea imprimir un archivo binario (por ejemplo, un archivo PostScript).|  
|-d|Especifica que el archivo de datos se debe enviar antes el archivo de control. Utilice este parámetro si la impresora requiere que se envían en primer lugar, el archivo de datos. Para obtener más información, consulte la documentación de la impresora.|  
|-x|Especifica que el **lpr** comando debe ser compatible con la Sun Microsystems (denominado SunOS) del sistema de operativo para las versiones hasta e incluyendo 4.1.4_u1.|  
|<FileName>|Especifica que se imprima el archivo (por nombre). Obligatorio.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  
## <a name="remarks"></a>Comentarios  
-   Para buscar el nombre de la impresora, abra la carpeta Impresoras.  
-   El **-S**, **-P**, **- C**, y **-J** los parámetros distinguen entre mayúsculas y minúsculas y debe escribirse en letras mayúsculas.  
## <a name="BKMK_examples"></a>Ejemplos  
En este ejemplo se muestra cómo imprimir el archivo de texto "Documento.txt" a la cola de impresión Laserprinter1 en un host LPD en 10.0.0.45:  
```  
lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt  
```  
En este ejemplo se muestra cómo imprimir el archivo PostScript de Adobe "PostScript_file.ps" a la cola de impresión Laserprinter1 en un host LPD en 10.0.0.45:  
```  
lpr -S 10.0.0.45 -P Laserprinter1 "-o l" PostScript_file.ps  
```  

#### <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
-   [Referencia de comandos de impresión](print-command-reference.md)  
