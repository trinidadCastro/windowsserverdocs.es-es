---
title: create volume raid
description: Tema de referencia para Create Volume RAID, que crea un volumen RAID-5 con tres o más discos dinámicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15a165439d0b19023fa5270eb372a17af8d558a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716934"
---
# <a name="create-volume-raid"></a>create volume raid

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea un volumen RAID-5 con tres o más discos dinámicos especificados.  

> [!IMPORTANT]  
> Este comando DiskPart no está disponible en ninguna edición de Windows Vista.

## <a name="syntax"></a>Sintaxis  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|           Parámetro           |                                                                                                                                                                                                                                              Descripción                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           ajusta\=<n>           | La cantidad de espacio en disco, en \(megabytes\)MB, que ocupará el volumen en cada disco. Si no se proporciona ningún tamaño, se creará\-el volumen RAID 5 más grande posible. El disco con el menor espacio libre contiguo disponible determina el tamaño del volumen RAID\-5 y se asigna la misma cantidad de espacio de cada disco. La cantidad real de espacio en disco que se puede\-usar en el volumen RAID 5 es menor que la cantidad combinada de espacio en disco porque se requiere parte del espacio en disco para la paridad. |
| disco\=<n>,<n><n>,\[,<n>,...\] |                                                                                                                                               Los discos dinámicos en los que se va\-a crear el volumen RAID 5. Necesita al menos tres discos dinámicos para crear un volumen RAID\-5. En cada disco se asigna una cantidad de espacio igual a **size\= ** .                                                                                                                                                |
|          alinea\=<n>           |                                                                                                                   Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con matrices de LUN \(\) de número de unidad lógica RAID de hardware para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano.                                                                                                                   |
|             noerr             |                                                                                                                                                 solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                                                                                                                                  |
  
## <a name="remarks"></a>Observaciones  
  
-   Después de crear el volumen, el foco cambiará automáticamente al nuevo volumen.  
  
## <a name="examples"></a>Ejemplos  
Para crear un volumen\-RAID 5 de 1000 megabytes de tamaño, con los discos 1, 2 y 3, escriba:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

