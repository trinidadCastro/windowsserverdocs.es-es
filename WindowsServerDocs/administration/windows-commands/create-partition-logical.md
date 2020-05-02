---
title: create partition logical
description: Tema de referencia de Create Partition Logical, que crea una partición lógica en una partición extendida existente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43d1c26d785fcecbb81a99c7484a15b3ea8b58a2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719234"
---
# <a name="create-partition-logical"></a>create partition logical

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea una partición lógica en una partición extendida existente. Este comando solo se puede usar en discos de registro de arranque maestro (MBR).

## <a name="syntax"></a>Sintaxis  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                                                                                                                       Descripción                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  ajusta\=<n>  |                                                                                                              Especifica el tamaño de la partición lógica en megabytes \(MB\), que debe ser menor que la partición extendida. Si no se proporciona ningún tamaño, la partición continuará hasta que no haya más espacio libre en la partición extendida.                                                                                                               |
| posición\=<n> | Especifica el desplazamiento en kilobytes \(KB\), en el que se crea la partición. El desplazamiento se redondea hacia arriba para llenar todo el tamaño del cilindro que se use. Si no se indica un desplazamiento, la partición se ubicará en la primera zona del disco que sea lo suficientemente grande como para albergarla. La partición es, como mínimo, en bytes como el número especificado por **size\=**. Si especifica un tamaño para la partición lógica, debe ser menor que la partición extendida. |
| alinea\=<n>  |                                                                                     Alinea todas las extensiones de volumen o partición con el límite de alineación más cercano. Normalmente se usa con matrices de LUN \(\) de número de unidad lógica RAID de hardware para mejorar el rendimiento.  <n>es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano.                                                                                      |
|    noerr    |                                                                                                                           solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                                                                                                           |
  
## <a name="remarks"></a>Observaciones  
  
-   Si no se especifican los parámetros de **tamaño** y **desplazamiento** , la partición lógica se crea en la extensión de disco más grande disponible en la partición extendida.  
  
-   Una vez creada la partición, el foco se desplaza automáticamente a la nueva partición lógica.  
  
-   Se debe seleccionar un disco MBR básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.  
  
## <a name="examples"></a>Ejemplos  
Para crear una partición lógica de 1000 megabytes de tamaño, en la partición extendida del disco seleccionado, escriba:  
  
```  
create partition logical size=1000  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

