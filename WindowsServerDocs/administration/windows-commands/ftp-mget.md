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
ms.openlocfilehash: e43bf8b6e7067a31b3ec51336b0b43845ab88f63
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438603"
---
# <a name="ftp-mget"></a>ftp: mget

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tipo de transferencia de la copia de archivos remoto en el equipo local mediante el archivo actual.   
## <a name="syntax"></a>Sintaxis  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |                        Descripción                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Especifica los archivos remotos que se copie en el equipo local. |

## <a name="BKMK_Examples"></a>Ejemplos  
Copie los archivos remotos **a.exe** y **b.exe** en el equipo local utilizando el tipo de transferencia de archivos actual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [ftp: ascii](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
