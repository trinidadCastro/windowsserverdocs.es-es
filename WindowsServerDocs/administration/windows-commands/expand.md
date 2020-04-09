---
title: expand
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ab4b9bcb5c9da2aeace71a8e535fa762d6d6e95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844878"
---
# <a name="expand"></a>expand

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

expande uno o más archivos comprimidos. Puede usar este comando para recuperar archivos comprimidos de discos de distribución.  
## <a name="syntax"></a>Sintaxis  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro  |                                                                                                                                                                   Descripción                                                                                                                                                                    |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /r      |                                                                                                                                                             cambia el nombre de los archivos expandidos.                                                                                                                                                              |
|   fuentes    |                                                                              Especifica los archivos que se van a expandir. El *origen* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos. Puede usar caracteres comodín ( **\\** \* o **?** ).                                                                               |
| de destino | Especifica la ubicación donde se van a expandir los archivos.<p>Si el *origen* consta de varios archivos y no especifica **/r**, el *destino* debe ser un directorio.<p>El *destino* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos.<p>Especificación de &#124; ruta de acceso del archivo de destino. |
|     /i      |                                                                                                   cambia el nombre de los archivos expandidos pero omite la estructura de directorios.<p>Este parámetro se aplica a: Windows Server 2008 R2 y Windows 7.                                                                                                    |
|     /d      |                                                                                                                              Muestra una lista de los archivos que se encuentran en el origen. No expande ni extrae los archivos.                                                                                                                              |
|     /f:     |                                                                                                                 Especifica los archivos de un archivo contenedor (. cab) que desea expandir. Puede usar caracteres comodín ( **\\** \* o **?** ).                                                                                                                 |
|     /?      |                                                                                                                                                       Muestra la Ayuda en el símbolo del sistema.                                                                                                                                                       |

## <a name="remarks"></a>Comentarios  
- Usar **Expand** en la consola de recuperación  
  El comando **Expand** , con diferentes parámetros, está disponible en la consola de recuperación. Para obtener más información acerca de la consola de recuperación, consulte el [artículo 314058](https://support.microsoft.com/kb/314058) en Microsoft Knowledge base.  
  ## <a name="additional-references"></a>Referencias adicionales  
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
