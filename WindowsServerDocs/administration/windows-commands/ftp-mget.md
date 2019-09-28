---
title: mget FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 666025c92b6fb1a612cbe7b83833557a8a7d5017
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376300"
---
# <a name="ftp-mget"></a>FTP: mget

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia los archivos remotos en el equipo local mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |                        Descripción                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Especifica los archivos remotos que se van a copiar en el equipo local. |

## <a name="BKMK_Examples"></a>Example  
Copie los archivos remotos a **. exe** y **b. exe** en el equipo local con el tipo de transferencia de archivos actual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
