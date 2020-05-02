---
title: mput_1 FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3fb654d5a2f44b9b63238abdbaee8d6a0294861
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725226"
---
# <a name="ftp-mput_1"></a>FTP: mput_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia los archivos locales en el equipo remoto mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
mput <LocalFile>[ ]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro  |                       Descripción                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Especifica el archivo local que se copiará en el equipo remoto. |

## <a name="examples"></a>Ejemplos  
Copie **Program1. exe** y **Program2. exe** en el equipo remoto mediante el tipo de transferencia de archivos actual.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
