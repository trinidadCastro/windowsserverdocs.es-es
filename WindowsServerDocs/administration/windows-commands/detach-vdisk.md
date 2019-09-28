---
title: Detach vDisk
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4850f9f17218178f210820dd4c6ca96fd918accc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378686"
---
# <a name="detach-vdisk"></a>Detach vDisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Detiene el disco duro virtual seleccionado \(VHD @ no__t-1 para que aparezca como una unidad de disco duro local en el equipo host. Cuando se desasocia un VHD, puede copiarlo en otras ubicaciones.  
  
> [!NOTE]  
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|Noerr|Se usa solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.  
  
## <a name="BKMK_Examples"></a>Example  
Para desasociar el disco duro virtual seleccionado, escriba:  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [Attach vDisk](attach-vdisk.md)  
  
-   [Compact vDisk](compact-vdisk.md)  
  
  
  
-   [detalles del vDisk](detail-vdisk.md)  
  
-   [expandir vDisk](expand-vdisk.md)  
  
-   [Merge vDisk](merge-vdisk.md)  
  
-   [seleccionar vDisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

