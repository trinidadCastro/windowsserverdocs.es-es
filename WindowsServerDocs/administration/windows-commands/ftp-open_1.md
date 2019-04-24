---
title: open_1 de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da7926914e2cbdbb4909093d90c33025ae5cd695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882476"
---
# <a name="ftp-open1"></a>FTP: open_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se conecta al servidor ftp especificada.   
## <a name="syntax"></a>Sintaxis  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|<computer>|Especifica el equipo remoto al que está intentando conectarse.|  
|[<Port>]|Especifica un número de puerto TCP para conectarse a un servidor ftp. De forma predeterminada, se usa el puerto TCP 21.|  
## <a name="remarks"></a>Comentarios  
Puede usar un nombre de equipo o dirección IP (en cuyo caso debe estar disponible un servidor DNS o el archivo de Hosts) para especificar **equipo**.  
## <a name="BKMK_Examples"></a>Ejemplos  
Conectarse al servidor ftp en **ftp.microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Conectarse al servidor ftp en **ftp.microsoft.com** que está escuchando en el puerto TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  