---
title: mget de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1160ec742dde318141da720bd35b7d60ab805bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888426"
---
# <a name="ftp-mget"></a>ftp: mget

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tipo de transferencia de la copia de archivos remoto en el equipo local mediante el archivo actual.   
## <a name="syntax"></a>Sintaxis  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|<remoteFile>|Especifica los archivos remotos que se copie en el equipo local.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Copie los archivos remotos **a.exe** y **b.exe** en el equipo local utilizando el tipo de transferencia de archivos actual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [ftp: ascii](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
