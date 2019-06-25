---
title: ftp mput_1
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd19a97246aa6155182cb055deceb4b5a5019f6c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438582"
---
# <a name="ftp-mput1"></a>ftp: mput_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tipo de transferencia de la copia de archivos local en el equipo remoto mediante el archivo actual.   
## <a name="syntax"></a>Sintaxis  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro  |                       Descripción                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Especifica el archivo local para copiar en el equipo remoto. |

## <a name="BKMK_Examples"></a>Ejemplos  
copia **Program1.exe** y **Program2.exe** al equipo remoto mediante el tipo de transferencia de archivos actual.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [ftp: ascii](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  