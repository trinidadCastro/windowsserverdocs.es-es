---
title: create partition extended
description: Comando comandos de Windows para crear partición extendida, que crea una partición extendida en el disco que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7071ed16d8ddbd1e37c9dd49bac8bb2b032b0b24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847078"
---
# <a name="create-partition-extended"></a>create partition extended

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partición extendida en el disco que tiene el foco. Este comando solo se puede usar en discos de registro de arranque maestro (MBR).

## <a name="syntax"></a>Sintaxis  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                             Descripción                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamaño\=<n>  |                                                  Especifica el tamaño de la partición en megabytes \(MB\). Si no se proporciona ningún tamaño, la partición continuará hasta que no haya más espacio libre en la partición extendida.                                                  |
| desplazamiento\=<n> |                     Especifica el desplazamiento en kilobytes \(KB\), en el que se crea la partición. Si no se proporciona ningún desplazamiento, la partición se iniciará al principio del espacio libre en el disco que sea lo suficientemente grande como para contener la nueva partición.                      |
| alinear\=<n>  | Alinea todas las extensiones de partición con el límite de alineación más cercano. Normalmente se usa con el número de unidad lógica RAID de hardware \(matrices\) LUN para mejorar el rendimiento. <n> es el número de kilobytes \(\) KB desde el principio del disco hasta el límite de alineación más cercano. |
|    noerr    |                                 solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                 |
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear la partición, ésta recibe el foco automáticamente.  
  
-   Sólo es posible crear una partición extendida por disco.  
  
-   Este comando produce un error si se intenta crear una partición extendida dentro de otra partición extendida.  
  
-   Se debe crear una partición extendida para poder crear unidades lógicas.  
  
-   Se debe seleccionar un disco MBR básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para crear una partición extendida de 1000 megabytes de tamaño, escriba:  
  
```  
create partition extended size=1000  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

