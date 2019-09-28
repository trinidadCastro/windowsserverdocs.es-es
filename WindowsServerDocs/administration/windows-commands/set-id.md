---
title: Identificador de conjunto
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5b48cc701716412c4a79cedddb4458c57ba25ad5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384068"
---
# <a name="set-id"></a>Identificador de conjunto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando de identificador set de Diskpart cambia el campo tipo de partición para la partición que tiene el foco.  
  
> [!IMPORTANT]  
> Este comando está pensado para su uso por parte de los fabricantes de equipos originales \(OEMs @ no__t-1 únicamente. Cambiar los campos de tipo de partición con este parámetro puede hacer que el equipo no funcione o no pueda arrancar. A menos que sea un OEM o que tenga experiencia en discos GPT, no debe cambiar los campos de tipo de partición en discos GPT mediante este parámetro. En su lugar, use siempre el comando [Create Partition EFI](create-partition-efi.md) para crear particiones del sistema EFI, el comando [Create Partition MSR](create-partition-msr.md) para crear particiones reservadas de Microsoft y el comando [Create Partition Primary](create-partition-primary.md) sin el parámetro id en cree particiones primarias en discos GPT.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
| Parámetro |                                                                                                                                                                                                                                                                                                                                                                   Descripción                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       en el caso de los discos de registro de arranque maestro \(MBR @ no__t-1, especifica el nuevo valor para el campo de tipo, en formato hexadecimal, para la partición. Cualquier byte de tipo de partición se puede especificar con este parámetro, excepto en el tipo 0x42, que especifica una partición LDM. Tenga en cuenta que el 0x inicial se omite al especificar el tipo de partición hexadecimal.                                                                                                                                                                                                       |
|  <GUID>   | en el caso de la tabla de particiones GUID \(gpt @ no__t-1 disks, especifica el nuevo valor GUID para el campo Type de la partición. Los GUID reconocidos incluyen:<br /><br />-Partición del sistema EFI: c12a7328 @ no__t-0f81f @ no__t-111d2 @ no__t-2ba4b @ no__t-300a0c93ec93b<br />-Partición de datos básica: ebd0a0a2 @ no__t-0b9e5 @ no__t-14433 @ no__t-287c0 @ no__t-368b6b72699c7<br /><br />Cualquier GUID de tipo de partición se puede especificar con este parámetro, excepto los siguientes:<br /><br />-Microsoft Reserved Partition: e3c9e316 @ no__t-00b5c @ no__t-14db8 @ no__t-2817d @ no__t-3f92df00215ae<br />-Partición de metadatos LDM en un disco dinámico: 5808c8aa @ no__t-07e8f @ no__t-142e0 @ no__t-285d2 @ no__t-3e1e90434cfb3<br />-Partición de datos LDM en un disco dinámico: af9b60a0 @ no__t-01431 @ no__t-14f62 @ no__t-2bc68 @ no__t-33311714a69ad<br />-Partición de metadatos del clúster: db97dba9 @ no__t-00840 @ no__t-14bae @ no__t-297f0 @ no__t-3ffb9a327c7e1 |
| estima  |                                                                fuerza el desmontaje del sistema de archivos en el volumen antes de cambiar el tipo de partición. Al ejecutar el comando **set ID** , DiskPart intenta bloquear y desmontar el sistema de archivos en el volumen. Si no se especifica **override** y se produce un error en la llamada para bloquear el sistema de archivos @no__t ejemplo-1Para, porque hay un identificador abierto @ no__t-2, se producirá un error en la operación. Cuando se especifica **override** , DiskPart fuerza el desmontaje incluso si se produce un error en la llamada para bloquear el sistema de archivos, y los identificadores abiertos del volumen dejarán de ser válidos.<br /><br />Este comando solo está disponible para Windows 7 y Windows Server 2008 R2.                                                                 |
|   Noerr   |                                                                                                                                                                                                                                                                    Se usa solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>Comentarios  
  
-   Aparte de las limitaciones mencionadas anteriormente, DiskPart no comprueba la validez del valor que especifique \(except para asegurarse de que es un byte en formato hexadecimal o un GUID @ no__t-1.  
  
-   Este comando no funciona en discos dinámicos ni en particiones reservadas de Microsoft.  
  
## <a name="BKMK_examples"></a>Example  
Para establecer el campo Type en 0x07 y forzar el desmontaje del sistema de archivos, escriba:  
  
```  
set id=0x07 override  
```  
  
Para establecer que el campo de tipo sea una partición de datos básica, escriba:  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

