---
title: Seleccione vdisk
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a71a5c15c05a1e969d0720bc8e67e669d553f649
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852576"
---
# <a name="select-vdisk"></a>Seleccione vdisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

selecciona el disco duro virtual especificado \(VHD\) y cambia el foco a ella.  
  
> [!NOTE]  
> Este comando sólo es aplicable a Windows 7 y Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|file\=<full path>|Especifica la ruta de acceso y el nombre completo de un archivo de disco duro virtual existente.|  
|noerr|Se utiliza sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para desplazar el foco en el disco duro virtual denominado Test.vhd, tipo:  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [Desasociar vdisk](detach-vdisk.md)  
  
-   [vdisk detallado](detail-vdisk.md)  
  
-   [Expandir vdisk](expand-vdisk.md)  
  
-   [Combinar vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

