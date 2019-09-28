---
title: ASCII de FTP
description: 'Temas de comandos de Windows para FTP ASCII '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5ae0064f9c1679bb8b386271f042d589b158c73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376621"
---
# <a name="ftp-ascii"></a>FTP: ASCII

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece el tipo de transferencia de archivos en ASCII.   
## <a name="syntax"></a>Sintaxis  
```  
ascii  
```  
### <a name="parameters"></a>Parámetros  
ninguno  
## <a name="remarks"></a>Comentarios  
- El tipo de transferencia de archivo predeterminado es ASCII.  
- En el modo ASCII, se realizan las conversiones de caracteres a y desde el juego de caracteres estándar de red. Por ejemplo, los caracteres de fin de línea se convierten según sea necesario, en función del sistema operativo de destino.  
- **FTP** admite tipos de transferencia de archivos de imagen binaria y ASCII. Use ASCII al transferir archivos de texto. Para obtener más información sobre la transferencia de archivos binarios, vea **FTP: Binary** en referencias adicionales.  
  ## <a name="BKMK_Examples"></a>Example  
  Establezca el tipo de transferencia de archivo en ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [FTP: binario](ftp-binary.md)  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
