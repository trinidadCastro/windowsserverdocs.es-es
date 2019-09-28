---
title: crear franja de volumen
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46c1367b5667294a7a9df742861a011090e7a337
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379137"
---
# <a name="create-volume-stripe"></a>crear franja de volumen

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un volumen seccionado mediante dos o más discos dinámicos especificados.  
  
> [!IMPORTANT]  
> en Windows Vista, este comando DiskPart solo está disponible en las ediciones Windows Vista Ultimate, Windows Vista Enterprise y Windows Vista Business.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|         Parámetro         |                                                                                                                            Descripción                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         Size @ no__t-0 @ no__t-1         |             La cantidad de espacio en disco, en megabytes @no__t 0MB @ no__t-1, que el volumen ocupará en cada disco. Si no se proporciona ningún tamaño, el nuevo volumen ocupará el espacio libre restante en el disco más pequeño y una cantidad igual de espacio en cada disco subsiguiente.             |
| Disk @ no__t-0 @ no__t-1, <n> @ no__t-3, <n>,... \] |                                  Los discos dinámicos en los que se crea el volumen seccionado. Necesita al menos dos discos dinámicos para crear un volumen seccionado. En cada disco se asigna una cantidad de espacio igual a **size @ no__t-1 @ no__t-2** .                                   |
|        align @ no__t-0 @ no__t-1         | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con el número de unidad lógica RAID de hardware \(LUN @ no__t-1 para mejorar el rendimiento. *n* es el número de kilobytes \( KB @ no__t-2 desde el principio del disco hasta el límite de alineación más cercano. |
|           Noerr           |                               Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                |
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco se desplaza automáticamente al nuevo volumen.  
  
## <a name="BKMK_examples"></a>Example  
Para crear un volumen seccionado de 1000 megabytes de tamaño, en los discos 1 y 2, escriba:  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

