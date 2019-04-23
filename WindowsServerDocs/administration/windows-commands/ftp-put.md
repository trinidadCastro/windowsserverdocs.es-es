---
title: put de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3307ba71e7b3c8b4113f9ed29ab06660dafa5f6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868746"
---
# <a name="ftp-put"></a>FTP: put

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un archivo local en el equipo remoto mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|<LocalFile>|Especifica el archivo local para copiar.|  
|[<remoteFile>]|Especifica el nombre que se usará en el equipo remoto.|  
## <a name="remarks"></a>Comentarios  
-   El **colocar** comando es idéntico a la **enviar** comando.  
-   Si *archivoRemoto* no se especifica, el archivo tendrá el *archivoLocal* nombre.  
## <a name="BKMK_Examples"></a>Ejemplos  
Copie el archivo local **test.txt** y asígnele el nombre **test1.txt** en el equipo remoto.  
```  
put test.txt test1.txt  
```  
Copie el archivo local **program.exe** al equipo remoto.  
```  
put program.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [ftp: ascii](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
