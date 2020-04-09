---
title: seleccionar vDisk
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65e186413bebbf467cd4c2033d274badd1fbea80
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834748"
---
# <a name="select-vdisk"></a>seleccionar vDisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

selecciona el disco duro virtual especificado \(VHD\) y desplaza el foco a él.  
  
> [!NOTE]  
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|<full path> de\=de archivos|Especifica la ruta de acceso completa y el nombre de un archivo VHD existente.|  
|noerr|Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para desplazar el foco al VHD denominado test. vhd, escriba:  
  
```  
select vdisk file=c:\test\test.vhd  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [Attach vDisk](attach-vdisk.md)  
  
-   [Compact vDisk](compact-vdisk.md)  
  
  
  
-   [Detach vDisk](detach-vdisk.md)  
  
-   [detalles del vDisk](detail-vdisk.md)  
  
-   [expandir vDisk](expand-vdisk.md)  
  
-   [Merge vDisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

