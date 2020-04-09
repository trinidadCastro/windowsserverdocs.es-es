---
title: LCD de FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d7e2e6fc9f6af7655381bfb802dc190e79365bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843408"
---
# <a name="ftp-lcd"></a>FTP: LCD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el directorio de trabajo en el equipo local. De forma predeterminada, el directorio de trabajo es el directorio en el que se inició **FTP** .   
## <a name="syntax"></a>Sintaxis  
```  
lcd [<directory>]  
```  
#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<directory>]|Especifica el directorio del equipo local al que se va a cambiar. Si no se especifica *Directory* , el directorio de trabajo actual se cambia al directorio predeterminado.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Cambie el directorio de trabajo en el equipo local a **C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
