---
title: crear partición lógica
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f18048272eda710f7cb53a631ddeda81784a56b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378893"
---
# <a name="create-partition-logical"></a>crear partición lógica

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea una partición lógica en una partición extendida existente. Este comando solo se puede usar en discos de registro de arranque maestro \(MBR @ no__t-1.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                                                                                                                       Descripción                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Size @ no__t-0 @ no__t-1  |                                                                                                              Especifica el tamaño de la partición lógica en megabytes \(MB @ no__t-1, que debe ser menor que la partición extendida. Si no se proporciona ningún tamaño, la partición continuará hasta que no haya más espacio libre en la partición extendida.                                                                                                               |
| offset @ no__t-0 @ no__t-1 | Especifica el desplazamiento en kilobytes \(KB @ no__t-1, en el que se crea la partición. El desplazamiento se redondea hacia arriba para llenar todo el tamaño del cilindro que se use. Si no se proporciona ningún desplazamiento, la partición se coloca en la primera extensión del disco que sea lo suficientemente grande como para almacenarla. La partición tiene al menos un valor de Long en bytes que el número especificado por **size @ no__t-1 @ no__t-2**. Si especifica un tamaño para la partición lógica, debe ser menor que la partición extendida. |
| align @ no__t-0 @ no__t-1  |                                                                                     Alinea todas las extensiones de volumen o partición con el límite de alineación más cercano. Normalmente se usa con el número de unidad lógica RAID de hardware \(LUN @ no__t-1 para mejorar el rendimiento.  <n> es el número de kilobytes \( KB @ no__t-2 desde el principio del disco hasta el límite de alineación más cercano.                                                                                      |
|    Noerr    |                                                                                                                           Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                                                                                                           |
  
## <a name="remarks"></a>Comentarios  
  
-   Si no se especifican los parámetros de **tamaño** y **desplazamiento** , la partición lógica se crea en la extensión de disco más grande disponible en la partición extendida.  
  
-   Una vez creada la partición, el foco se desplaza automáticamente a la nueva partición lógica.  
  
-   Se debe seleccionar un disco MBR básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.  
  
## <a name="BKMK_examples"></a>Example  
Para crear una partición lógica de 1000 megabytes de tamaño, en la partición extendida del disco seleccionado, escriba:  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

