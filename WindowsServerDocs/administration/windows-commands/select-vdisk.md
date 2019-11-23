---
title: seleccionar vDisk
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bfa6450d1704cde1e5ff2a50a8e3b61a30d0766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384190"
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
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|<full path> de\=de archivos|Especifica la ruta de acceso completa y el nombre de un archivo VHD existente.|  
|Noerr|Se usa solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|  
  
## <a name="BKMK_examples"></a>Example  
Para desplazar el foco al VHD denominado test. vhd, escriba:  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [Attach vDisk](attach-vdisk.md)  
  
-   [Compact vDisk](compact-vdisk.md)  
  
  
  
-   [Detach vDisk](detach-vdisk.md)  
  
-   [detalles del vDisk](detail-vdisk.md)  
  
-   [expandir vDisk](expand-vdisk.md)  
  
-   [Merge vDisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

