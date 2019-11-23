---
title: select disk
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d9078242264b01ee4bc24dc590df24b1e53e548
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371082"
---
# <a name="select-disk"></a>select disk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

selecciona el disco especificado y desplaza el foco a él.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> Los parámetros **<disk path>** , **System**y **Next** solo están disponibles en Windows 7 y Windows Server 2008 R2.  
  
## <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                                                                                                            Descripción                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | Especifica el número del disco en el que se va a recibir el foco. Puede ver los números de todos los discos del equipo mediante el comando **List Disk** en Diskpart. **Nota:** Al configurar sistemas con varios discos, no use **Select disk\=0** para especificar el disco del sistema. El equipo puede reasignar los números de disco al reiniciar y los distintos equipos con la misma configuración de disco pueden tener números de disco diferentes. |
| <disk path> |                                                                                                                 Especifica la ubicación del disco para recibir el foco, por ejemplo, **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . Para ver la ruta de acceso de ubicación de un disco, selecciónelo y, a continuación, escriba **detail Disk**.                                                                                                                  |
|   Sistema    |                                 En los equipos BIOS, especifica que el disco 0 recibe el foco. En los equipos EFI, el disco que contiene la partición del sistema EFI \(ESP\) que se usa para el arranque actual recibe el foco. En los equipos EFI, el comando producirá un error si no hay ningún ESP, si hay más de un ESP, o si el equipo se inicia desde Entorno de preinstalación de Windows \(\)de Windows PE.                                  |
|    siguiente     |                                                                                                                                     Una vez que se selecciona un disco, este comando recorre en iteración todos los discos de la lista de discos. Al ejecutar este comando, el siguiente disco de la lista recibirá el foco.                                                                                                                                      |
  
## <a name="BKMK_examples"></a>Example  
Para desplazar el foco al disco 1, escriba:  
  
```  
select disk=1  
```  
  
Para seleccionar un disco mediante la ruta de acceso de ubicación, escriba:  
  
```  
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)  
```  
  
Para desplazar el foco al disco del sistema, escriba:  
  
```  
select disk=system  
```  
  
Para desplazar el foco al siguiente disco del equipo, escriba:  
  
```  
select disk=next  
```  
  
#### <a name="additional-references"></a>referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

