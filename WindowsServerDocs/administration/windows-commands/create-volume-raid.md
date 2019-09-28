---
title: Create Volume RAID
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a3c13cb5b78ae3e771b461a35a7130a48e7ec01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378865"
---
# <a name="create-volume-raid"></a>Create Volume RAID

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un volumen RAID @ no__t-05 con tres o más discos dinámicos especificados.  
  
> [!IMPORTANT]  
> Este comando DiskPart no está disponible en ninguna edición de Windows Vista.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|           Parámetro           |                                                                                                                                                                                                                                              Descripción                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           Size @ no__t-0 @ no__t-1           | La cantidad de espacio en disco, en megabytes @no__t 0MB @ no__t-1, que el volumen ocupará en cada disco. Si no se proporciona ningún tamaño, se creará el volumen RAID @ no__t-05 más grande posible. El disco con el menor espacio libre contiguo disponible determina el tamaño del volumen RAID @ no__t-05 y se asigna la misma cantidad de espacio de cada disco. La cantidad real de espacio en disco que se puede usar en el volumen RAID @ no__t-05 es menor que la cantidad combinada de espacio en disco porque se requiere parte del espacio en disco para la paridad. |
| disco @ no__t-0 @ no__t-1, <n>, <n> @ no__t-4, <n>,... \] |                                                                                                                                               Los discos dinámicos en los que se va a crear el volumen RAID @ no__t-05. Necesita al menos tres discos dinámicos para crear un volumen RAID @ no__t-05. En cada disco se asigna una cantidad de espacio igual a **size @ no__t-1 @ no__t-2** .                                                                                                                                                |
|          align @ no__t-0 @ no__t-1           |                                                                                                                   Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con el número de unidad lógica RAID de hardware \(LUN @ no__t-1 para mejorar el rendimiento. *n* es el número de kilobytes \( KB @ no__t-2 desde el principio del disco hasta el límite de alineación más cercano.                                                                                                                   |
|             Noerr             |                                                                                                                                                 Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                                                                                                                                  |
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco se desplaza automáticamente al nuevo volumen.  
  
## <a name="BKMK_examples"></a>Example  
Para crear un volumen RAID @ no__t-05 de 1000 megabytes de tamaño, con los discos 1, 2 y 3, escriba:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

