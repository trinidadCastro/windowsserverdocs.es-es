---
title: create partition extended
description: Tema de referencia de Create Partition Extended, que crea una partición extendida en el disco que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b8af7247a9084b722f5b510df1d6af4622fc4ac2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719246"
---
# <a name="create-partition-extended"></a>create partition extended

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea una partición extendida en el disco que tiene el foco. Este comando solo se puede usar en discos de registro de arranque maestro (MBR).

## <a name="syntax"></a>Sintaxis  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                             Descripción                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  ajusta\=<n>  |                                                  Especifica el tamaño de la partición en megabytes \(MB\). Si no se proporciona ningún tamaño, la partición continuará hasta que no haya más espacio libre en la partición extendida.                                                  |
| posición\=<n> |                     Especifica el desplazamiento en kilobytes \(KB\), en el que se crea la partición. Si no se proporciona ningún desplazamiento, la partición se iniciará al principio del espacio libre en el disco que sea lo suficientemente grande como para contener la nueva partición.                      |
| alinea\=<n>  | Alinea todas las extensiones de partición con el límite de alineación más cercano. Normalmente se usa con matrices de LUN \(\) de número de unidad lógica RAID de hardware para mejorar el rendimiento. <n>es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano. |
|    noerr    |                                 solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                 |
  
## <a name="remarks"></a>Observaciones  
  
-   Después de crear la partición, ésta recibe el foco automáticamente.  
  
-   Sólo es posible crear una partición extendida por disco.  
  
-   Este comando produce un error si se intenta crear una partición extendida dentro de otra partición extendida.  
  
-   Se debe crear una partición extendida para poder crear unidades lógicas.  
  
-   Se debe seleccionar un disco MBR básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.  
  
## <a name="examples"></a>Ejemplos  
Para crear una partición extendida de 1000 megabytes de tamaño, escriba:  
  
```  
create partition extended size=1000  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

