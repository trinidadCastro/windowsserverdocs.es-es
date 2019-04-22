---
title: binario de FTP
description: Tema de los comandos de Windows para el binario de ftp
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cadd59bff3bd2acf5c6d700caef66ca5c871b523
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821926"
---
# <a name="ftp-binary"></a>FTP: binario

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece el tipo de transferencia de archivos como binario.   
## <a name="syntax"></a>Sintaxis  
```  
binary  
```  
### <a name="parameters"></a>Parámetros  
ninguno  
## <a name="remarks-optional-section"></a>Comentarios <optional section>  
**FTP** es compatible con ASCII y tipos de transferencia de archivos de imagen binarios. Usa el archivo binario al transferir los archivos ejecutables. En modo binario, los archivos se transfieren en unidades de un byte. Para obtener más información acerca de la transferencia de archivos ASCII, vea **ftp: ascii** en referencias adicionales.  
## <a name="BKMK_Examples"></a>Ejemplos  
Establezca el tipo de transferencia de archivos al archivo binario.  
```  
binary  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [ftp: ascii](ftp-ascii.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
