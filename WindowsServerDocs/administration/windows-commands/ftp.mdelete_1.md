---
title: mdelete_1 de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b342f9d728e91085d5edf2f8e1ece00b48bec8d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438320"
---
# <a name="ftp-mdelete1"></a>ftp: mdelete_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina los archivos en el equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mdelete <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |             Descripción              |
|--------------|--------------------------------------|
| <remoteFile> | Especifica el archivo remoto para eliminar. |

## <a name="BKMK_Examples"></a>Ejemplos  
eliminar archivos remotos **a.exe** y **b.exe**.  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
