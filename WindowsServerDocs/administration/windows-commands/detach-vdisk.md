---
title: Desasociar vdisk
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0b6a1ecd3d787506c89f120bed204cc30e6d68d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822736"
---
# <a name="detach-vdisk"></a>Desasociar vdisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Detiene el disco duro virtual seleccionado \(VHD\) aparezca como una unidad de disco duro local en el equipo host. Cuando se desasocia un VHD, puede copiarlo en otras ubicaciones.  
  
> [!NOTE]  
> Este comando sólo es aplicable a Windows 7 y Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|noerr|Se utiliza sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   Un disco duro virtual debe estar seleccionado y desconectado para que esta operación se realice correctamente. Use la **seleccione vdisk** comando para seleccionar un disco duro virtual y desplace el foco a ella.  
  
## <a name="BKMK_Examples"></a>Ejemplos  
Para desasociar el disco duro virtual seleccionado, escriba:  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [vdisk detallado](detail-vdisk.md)  
  
-   [Expandir vdisk](expand-vdisk.md)  
  
-   [Combinar vdisk](merge-vdisk.md)  
  
-   [select vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

