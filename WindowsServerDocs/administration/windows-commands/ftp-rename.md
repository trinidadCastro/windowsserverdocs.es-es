---
title: cambio de nombre de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d1a15f038017444c7654a44748bfd22be8e487
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438382"
---
# <a name="ftp-rename"></a>FTP: cambiar el nombre

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el nombre de los archivos remotos.   
## <a name="syntax"></a>Sintaxis  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Parámetros  

|   Parámetro   |                 Descripción                 |
|---------------|---------------------------------------------|
|  <FileName>   | Especifica el archivo que desea cambiar el nombre. |
| <NewFileName> |        Especifica el nuevo nombre de archivo.         |

## <a name="BKMK_Examples"></a>Ejemplos  
el nombre del archivo remoto **example.txt** a **example1.txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
