---
title: LCD de FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47058cc62e4e87d1fcd951ec3b4a7885eeef2ae2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376308"
---
# <a name="ftp-lcd"></a>FTP: LCD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el directorio de trabajo en el equipo local. De forma predeterminada, el directorio de trabajo es el directorio en el que se inició **FTP** .   
## <a name="syntax"></a>Sintaxis  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<directory>]|Especifica el directorio del equipo local al que se va a cambiar. Si no se especifica *Directory* , el directorio de trabajo actual se cambia al directorio predeterminado.|  
## <a name="BKMK_Examples"></a>Example  
Cambie el directorio de trabajo en el equipo local a **C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
