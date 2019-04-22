---
title: expand
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5df078af23c77f54ccb2da83b1057c5d7042593a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825966"
---
# <a name="expand"></a>expand

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

expande uno o varios archivos comprimidos. Puede usar este comando para recuperar archivos comprimidos de discos de distribución.  
## <a name="syntax"></a>Sintaxis  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|/r|cambia el nombre de los archivos expandidos.|  
|Código fuente|Especifica los archivos para expandir. *Origen* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos. Puede usar caracteres comodín (**\*** o **?**).|  
|Destino|Especifica dónde están los archivos que debe expandirse.<br /><br />Si *origen* consta de varios archivos y no se especifica **/r**, *destino* debe ser un directorio.<br /><br />*Destino* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos.<br /><br />Archivo de destino &#124; especificación de ruta de acceso.|  
|/i|cambia el nombre de los archivos expandidos pero omite la estructura de directorios.<br /><br />Este parámetro se aplica a:  Windows Server 2008 R2 y Windows 7.|  
|/d|Muestra una lista de archivos en la ubicación de origen. Expande ni extrae los archivos.|  
|/f:|Especifica los archivos en un archivo contenedor (.cab) que desea expandir. Puede usar caracteres comodín (**\*** o **?**).|  
|/?|Muestra la ayuda en el símbolo del sistema.|  
## <a name="remarks"></a>Comentarios  
-   Uso de **expanda** en la consola de recuperación  
    El **expanda** comando, con diferentes parámetros, está disponible en la consola de recuperación. Para obtener más información acerca de la consola de recuperación, consulte [artículo 314058](https://support.microsoft.com/kb/314058) en Microsoft Knowledge Base.  
## <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
