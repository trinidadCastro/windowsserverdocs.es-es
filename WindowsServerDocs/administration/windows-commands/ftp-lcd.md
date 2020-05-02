---
title: LCD de FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 646cbfe3feadb63388694218758dae165ffb49c4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725262"
---
# <a name="ftp-lcd"></a>FTP: LCD

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

cambia el directorio de trabajo en el equipo local. De forma predeterminada, el directorio de trabajo es el directorio en el que se inició **FTP** .   
## <a name="syntax"></a>Sintaxis  
```  
lcd [<directory>]  
```  
#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<directory>]|Especifica el directorio del equipo local al que se va a cambiar. Si no se especifica *Directory* , el directorio de trabajo actual se cambia al directorio predeterminado.|  
## <a name="examples"></a>Ejemplos  
Cambie el directorio de trabajo en el equipo local a **C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
