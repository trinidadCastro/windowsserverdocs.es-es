---
title: mget FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72b45f84622fcd6abf7a743f606fb514def584cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843358"
---
# <a name="ftp-mget"></a>FTP: mget

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia los archivos remotos en el equipo local mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
mget <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |                        Descripción                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Especifica los archivos remotos que se van a copiar en el equipo local. |

## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Copie los archivos remotos a **. exe** y **b. exe** en el equipo local con el tipo de transferencia de archivos actual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
