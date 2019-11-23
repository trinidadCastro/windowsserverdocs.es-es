---
title: crear partición extendida
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21620da46be0e1375f320172e7ccfe2edc338114
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378914"
---
# <a name="create-partition-extended"></a>crear partición extendida

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea una partición extendida en el disco que tiene el foco. Puede usar este comando solo en los discos de registro de arranque maestro \(MBR\).  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                             Descripción                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamaño\=<n>  |                                                  Especifica el tamaño de la partición en megabytes \(MB\). Si no se proporciona ningún tamaño, la partición continuará hasta que no haya más espacio libre en la partición extendida.                                                  |
| desplazamiento\=<n> |                     Especifica el desplazamiento en kilobytes \(KB\), en el que se crea la partición. Si no se proporciona ningún desplazamiento, la partición se iniciará al principio del espacio libre en el disco que sea lo suficientemente grande como para contener la nueva partición.                      |
| alinear\=<n>  | Alinea todas las extensiones de partición con el límite de alineación más cercano. Normalmente se usa con el número de unidad lógica RAID de hardware \(matrices\) LUN para mejorar el rendimiento. <n> es el número de kilobytes \(\) KB desde el principio del disco hasta el límite de alineación más cercano. |
|    Noerr    |                                 solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                 |
  
## <a name="remarks"></a>Observaciones  
  
-   Una vez creada la partición, el foco se desplaza automáticamente a la nueva partición.  
  
-   Solo se puede crear una partición extendida por disco.  
  
-   Este comando produce un error si intenta crear una partición extendida dentro de otra partición extendida.  
  
-   Debe crear una partición extendida para poder crear unidades lógicas.  
  
-   Se debe seleccionar un disco MBR básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.  
  
## <a name="BKMK_examples"></a>Example  
Para crear una partición extendida de 1000 megabytes de tamaño, escriba:  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

