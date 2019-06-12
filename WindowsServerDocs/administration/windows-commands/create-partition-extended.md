---
title: Crear partición extendida
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0a1cca93a064cfb6e5c18f4a472ea837b922d07b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434193"
---
# <a name="create-partition-extended"></a>Crear partición extendida

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea una partición extendida en el disco con el foco. Puede usar este comando solo en el registro de arranque maestro \(MBR\) discos.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                             Descripción                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                                                  Especifica el tamaño de la partición en megabytes \(MB\). Si no se especifica tamaño, la partición continuará mientras haya espacio libre en la partición extendida.                                                  |
| offset\=<n> |                     Especifica el desplazamiento en kilobytes \(KB\), que se crea la partición. Si se indica ningún desplazamiento, la partición comenzará al principio del espacio libre en el disco que es lo suficientemente grande como para contener la nueva partición.                      |
| Alinear\=<n>  | Alinea todas las extensiones de partición para el límite de alineación más cercano. Se utiliza normalmente con el número de unidad lógica de RAID de hardware \(LUN\) matrices para mejorar el rendimiento. <n> es el número de kilobytes \(KB\) desde el principio del disco para el límite de alineación más cercano. |
|    noerr    |                                 sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.                                 |
  
## <a name="remarks"></a>Comentarios  
  
-   Una vez creada la partición, ésta recibe el foco automáticamente.  
  
-   Puede crearse solo una partición extendida por disco.  
  
-   Este comando produce un error si intenta crear una partición extendida en otra partición extendida.  
  
-   Debe crear una partición extendida para poder crear unidades lógicas.  
  
-   Debe seleccionarse un disco MBR básico para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear una partición extendida de 1000 megabytes de tamaño, escriba:  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

