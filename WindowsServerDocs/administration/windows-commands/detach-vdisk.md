---
title: detach vdisk
description: Tema de referencia sobre detach vDisk, que detiene el disco duro virtual (VHD) seleccionado para que aparezca como una unidad de disco duro local en el equipo host.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5e64559650597eb8d15e28075f74704fdf338a6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716667"
---
# <a name="detach-vdisk"></a>detach vdisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Detiene el disco duro virtual (VHD) seleccionado para que no aparezca como una unidad de disco duro local en el equipo host. Cuando se desasocia un VHD, puede copiarlo en otras ubicaciones.  
  
> [!NOTE]  
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
detach vdisk [noerr]  
```  
  
#### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|noerr|Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|  
  
## <a name="remarks"></a>Observaciones  
  
-   Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.  
  
## <a name="examples"></a>Ejemplos  
Para desasociar el disco duro virtual seleccionado, escriba:  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [Compact vDisk](compact-vdisk.md)  

-   [detalles del vDisk](detail-vdisk.md)  
  
-   [expandir vDisk](expand-vdisk.md)  
  
-   [Merge vDisk](merge-vdisk.md)  
  
-   [seleccionar vDisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

