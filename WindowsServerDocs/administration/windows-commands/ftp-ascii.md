---
title: FTP ascii
description: 'Tema de los comandos de Windows para ascii de ftp '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b7bcca3f29cec8ff5c30256dfd123acc7fbb804d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849196"
---
# <a name="ftp-ascii"></a>ftp: ascii

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece el tipo de transferencia de archivos como ASCII.   
## <a name="syntax"></a>Sintaxis  
```  
ascii  
```  
### <a name="parameters"></a>Parámetros  
ninguno  
## <a name="remarks"></a>Comentarios  
-   El tipo de transferencia de archivo predeterminado es ASCII.  
-   En modo ASCII, se realizan las conversiones de caracteres a y desde el juego de caracteres estándar de la red. Por ejemplo, los caracteres de fin de línea se convierten según sea necesario, según el sistema operativo de destino.  
-   **FTP** es compatible con ASCII y tipos de transferencia de archivos de imagen binarios. Usar ASCII al transferir los archivos de texto. Para obtener más información acerca de la transferencia de archivos binarios, vea **ftp: binario** en referencias adicionales.  
## <a name="BKMK_Examples"></a>Ejemplos  
Establece el tipo de transferencia de archivos en ASCII.  
```  
ascii  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: binario](ftp-binary.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
