---
title: crear reflejo de volumen
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72ecc4e0ede163857c47c5b7013aacdd49719ac8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378872"
---
# <a name="create-volume-mirror"></a>crear reflejo de volumen

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un reflejo de volumen mediante el uso de los dos discos dinámicos especificados.  
  
> [!NOTE]  
> Este comando solo está disponible en Windows 7 y Windows Server 2008 R2.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|         Parámetro         |                                                                                                                                     Descripción                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         Size @ no__t-0 @ no__t-1         |                 Especifica la cantidad de espacio en disco, en megabytes \(MB @ no__t-1, que el volumen ocupará en cada disco. Si no se proporciona ningún tamaño, el nuevo volumen ocupará el espacio libre restante en el disco más pequeño y una cantidad igual de espacio en cada disco subsiguiente.                 |
| Disk @ no__t-0 @ no__t-1, <n> @ no__t-3, <n>,... \] |                       Especifica los discos dinámicos en los que se crea el volumen reflejado. Necesita dos discos dinámicos para crear un volumen reflejado. En cada disco se asigna una cantidad de espacio igual al tamaño especificado con el parámetro de **tamaño** .                        |
|        align @ no__t-0 @ no__t-1         | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Este parámetro se utiliza normalmente con matrices de número de unidad lógica RAID de hardware \(LUN @ no__t-1 para mejorar el rendimiento. *n* es el número de kilobytes \( KB @ no__t-2 desde el principio del disco hasta el límite de alineación más cercano. |
|           Noerr           |                                        Se usa solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart se cierre con un error.                                         |
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco se desplaza automáticamente al nuevo volumen.  
  
## <a name="BKMK_examples"></a>Example  
Para crear un volumen reflejado de 1000 megabytes de tamaño, en los discos 1 y 2, escriba:  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

