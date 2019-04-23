---
title: ftp lcd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: eac028c8aa675e680dedefcfe9f0b8da18ce7179
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826896"
---
# <a name="ftp-lcd"></a>ftp: lcd

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el directorio de trabajo en el equipo local. De forma predeterminada, el directorio de trabajo es el directorio en el que **ftp** se inició.   
## <a name="syntax"></a>Sintaxis  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<directory>]|Especifica el directorio en el equipo local que se va a cambiar. Si *directorio* no se especifica, se cambia el directorio de trabajo actual en el directorio predeterminado.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Cambie el directorio de trabajo en el equipo local para **C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
