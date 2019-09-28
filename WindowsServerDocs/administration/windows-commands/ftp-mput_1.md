---
title: mput_1 FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6308f7b47d58bc25d964944f96fbc83a26350962
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376208"
---
# <a name="ftp-mput_1"></a>FTP: mput_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia los archivos locales en el equipo remoto mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro  |                       Descripción                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Especifica el archivo local que se copiará en el equipo remoto. |

## <a name="BKMK_Examples"></a>Example  
Copie **Program1. exe** y **Program2. exe** en el equipo remoto mediante el tipo de transferencia de archivos actual.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
