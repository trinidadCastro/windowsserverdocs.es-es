---
title: ASCII de FTP
description: Tema de referencia de FTP ASCII
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa33637f43ef8d26635f36b40dbd21cfe2bff42e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725393"
---
# <a name="ftp-ascii"></a>FTP: ASCII

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Establece el tipo de transferencia de archivos en ASCII.   
## <a name="syntax"></a>Sintaxis  
```  
ascii  
```  
#### <a name="parameters"></a>Parámetros  
None  
## <a name="remarks"></a>Observaciones  
- El tipo de transferencia de archivo predeterminado es ASCII.  
- En el modo ASCII, se realizan las conversiones de caracteres a y desde el juego de caracteres estándar de red. Por ejemplo, los caracteres de fin de línea se convierten según sea necesario, en función del sistema operativo de destino.  
- **FTP** admite tipos de transferencia de archivos de imagen binaria y ASCII. Use ASCII al transferir archivos de texto. Para obtener más información sobre la transferencia de archivos binarios, vea **FTP: Binary** en referencias adicionales.  
  ## <a name="examples"></a>Ejemplos  
  Establezca el tipo de transferencia de archivo en ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [FTP: binario](ftp-binary.md)  
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
