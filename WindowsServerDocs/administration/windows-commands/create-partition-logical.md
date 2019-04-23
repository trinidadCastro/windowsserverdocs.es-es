---
title: Crear partición lógica
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6b347581773a086d525bb005edeca2efa31e1848
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886056"
---
# <a name="create-partition-logical"></a>Crear partición lógica

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea una partición lógica en una partición extendida existente. Solo puede usar este comando en el registro de arranque maestro \(MBR\) discos.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|size\=<n>|Especifica el tamaño de la partición lógica en megabytes \(MB\), que debe ser menor que la partición extendida. Si no se especifica tamaño, la partición continuará mientras haya espacio libre en la partición extendida.|  
|offset\=<n>|Especifica el desplazamiento en kilobytes \(KB\), que se crea la partición. El desplazamiento se redondea hacia arriba para llenar completamente el tamaño en cilindros que se usa. Si se indica ningún desplazamiento, la partición se coloca en la primera zona del disco que sea lo suficientemente grande como para albergarla. La partición es al menos tan larga en bytes, como el número especificado por **tamaño\=<n>**. Si especifica un tamaño de la partición lógica, debe ser menor que la partición extendida.|  
|Alinear\=<n>|Alinea todas las extensiones de volumen o partición para el límite de alineación más cercano. Se utiliza normalmente con el número de unidad lógica de RAID de hardware \(LUN\) matrices para mejorar el rendimiento.  <n> es el número de kilobytes \(KB\) desde el principio del disco para el límite de alineación más cercano.|  
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   Si el **tamaño** y **desplazamiento** no se especifican parámetros, se crea la partición lógica en la zona del disco más grande disponible en la partición extendida.  
  
-   Una vez creada la partición, el foco se desplaza automáticamente a la nueva partición lógica.  
  
-   Debe seleccionarse un disco MBR básico para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear una partición lógica de 1000 megabytes de tamaño, en la partición extendida del disco seleccionado, escriba:  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

