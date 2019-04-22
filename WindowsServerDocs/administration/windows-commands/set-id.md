---
title: Id. de conjunto
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f95490850acd263fb0b34007ac64a84c9a374865
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822056"
---
# <a name="set-id"></a>Id. de conjunto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando de Id. de conjunto de Diskpart cambia el campo de tipo de partición para la partición con el foco.  
  
> [!IMPORTANT]  
> Este comando está pensado para su uso por fabricantes de equipos originales \(OEM\) solo. Cambiar los campos de tipo de partición con este parámetro puede provocar que su equipo producirá un error o no se pueda arrancar. A menos que se encuentra un OEM o con experiencia en discos gpt, no debe cambiar los campos de tipo de partición en discos gpt mediante el uso de este parámetro. En su lugar, use siempre la [crear partición efi](create-partition-efi.md) comando para crear particiones de sistema EFI, el [crear partición msr](create-partition-msr.md) comando para crear particiones de Microsoft Reserved y el [crear partición principal](create-partition-primary.md) comando sin el parámetro ID para crear particiones primarias en discos gpt.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|<byte>|para el registro de arranque maestro \(MBR\) discos, especifica el nuevo valor para el campo de tipo, en formato hexadecimal, para la partición. Byte de tipo de partición puede especificarse con este parámetro, excepto el tipo 0 x 42, que especifica una partición LDM. Tenga en cuenta que el 0 x inicial se omite cuando se especifica el tipo de partición hexadecimal.|  
|<GUID>|para la tabla de particiones GUID \(gpt\) discos, especifica el nuevo valor GUID para el campo tipo de la partición. GUID reconocidos incluyen:<br /><br />-Partición de sistema EFI: c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Partición de datos básica: ebd0a0a2\-b9e5\-4433\-87c 0\-68b6b72699c7<br /><br />Cualquier tipo de partición GUID puede especificarse con este parámetro, excepto los siguientes:<br /><br />-Partición de Microsoft Reserved: e3c9e316\-0b5c\-4db8\-817D\-f92df00215ae<br />-Partición de metadatos LDM en un disco dinámico: 5808c8aa\-7e8f\-42e0\-85 d. 2\-e1e90434cfb3<br />-Partición de datos LDM en un disco dinámico: af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-Partición de metadatos del clúster: db97dba9\-0840\-4bae\-97f0\-ffb9a327c7e1|  
|invalidar|obliga al sistema de archivos en el volumen se desmonte antes de cambiar el tipo de partición. Al ejecutar el **Id. de conjunto** comando DiskPart intenta bloquear y desmontar el sistema de archivos en el volumen. Si **invalidar** no se especifica, y se produce un error en la llamada a bloquear el sistema de archivos \(por ejemplo, porque no hay un identificador abierto\), se producirá un error en la operación. Cuando **invalidar** se especifica, DiskPart fuerza el desmontaje incluso si se produce un error en la llamada a bloquear el sistema de archivos y los identificadores abiertos en el volumen dejarán de ser válidos.<br /><br />Este comando solo está disponible para Windows 7 y Windows Server 2008 R2.|  
|noerr|Se utiliza sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   Aparte de las limitaciones que se mencionó anteriormente, DiskPart no comprueba la validez del valor que especifique \(excepto para asegurarse de que es un byte en formato hexadecimal o un GUID\).  
  
-   Este comando no funciona en discos dinámicos o en Microsoft Reserved particiones.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para establecer el campo de tipo 0 x 07 y forzar el sistema de archivos para desmontar, escriba:  
  
```  
set id=0x07 override  
```  
  
Para establecer el campo de tipo sea una partición de datos básica, escriba:  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

