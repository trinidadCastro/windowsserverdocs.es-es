---
title: ASCII de FTP
description: Temas de comandos de Windows para FTP ASCII
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4930d0d726acaa222f802d7aaef59578030db5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843788"
---
# <a name="ftp-ascii"></a>FTP: ASCII

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece el tipo de transferencia de archivos en ASCII.   
## <a name="syntax"></a>Sintaxis  
```  
ascii  
```  
#### <a name="parameters"></a>Parámetros  
ninguno  
## <a name="remarks"></a>Comentarios  
- El tipo de transferencia de archivo predeterminado es ASCII.  
- En el modo ASCII, se realizan las conversiones de caracteres a y desde el juego de caracteres estándar de red. Por ejemplo, los caracteres de fin de línea se convierten según sea necesario, en función del sistema operativo de destino.  
- **FTP** admite tipos de transferencia de archivos de imagen binaria y ASCII. Use ASCII al transferir archivos de texto. Para obtener más información sobre la transferencia de archivos binarios, vea **FTP: Binary** en referencias adicionales.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Example  
  Establezca el tipo de transferencia de archivo en ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [FTP: binario](ftp-binary.md)  
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
