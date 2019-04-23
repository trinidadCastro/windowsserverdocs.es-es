---
title: get de FTP
description: Obtiene el tema de los comandos de Windows para ftp
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 798317f3921cd0e5ff12b69b972e2ea423fa6b3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816736"
---
# <a name="ftp-get"></a>FTP: obtener

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un archivo remoto en el equipo local utilizando el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|<remoteFile>|Especifica el archivo remoto para copiar.|  
|[<LocalFile>]|Especifica el nombre del archivo que se use en el equipo local. Si *archivoLocal* no se especifica, el archivo tendrá el *archivoRemoto* nombre.|  
## <a name="remarks"></a>Comentarios  
El **obtener** comando es idéntico a la **recv** comando.  
## <a name="BKMK_Examples"></a>Ejemplos  
copia **test.txt** en el equipo local utilizando el tipo de transferencia de archivos actual.  
```  
get test.txt  
```  
copia **test.txt** en el equipo local como **test1.txt** tipo de transferencia mediante el archivo actual.  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [ftp: ascii](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  