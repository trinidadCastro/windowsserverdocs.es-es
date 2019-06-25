---
title: recepción de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bfd68dcb745ebf7ef239883aa1c5322241b32df
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438431"
---
# <a name="ftp-recv"></a>ftp: recv

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un archivo remoto en el equipo local utilizando el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parámetros  

|   Parámetro   |                   Descripción                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Especifica el archivo remoto para copiar.        |
| [<LocalFile>] | Especifica el nombre que se usará en el equipo local. |

## <a name="remarks"></a>Comentarios  
- El **recv** comando es idéntico a la **obtener** comando.  
- Si *archivoLocal* no se especifica, el archivo tendrá el *archivoRemoto* nombre.  
  ## <a name="BKMK_Examples"></a>Ejemplos  
  copia **test.txt** en el equipo local utilizando el tipo de transferencia de archivos actual.  
  ```  
  recv test.txt  
  ```  
  copia **test.txt** en el equipo local como **test1.txt** tipo de transferencia mediante el archivo actual.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [ftp: ascii](ftp-ascii.md)  
- [FTP: binario](ftp-binary.md)  
- [ftp: get](ftp-get.md)  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  