---
title: Crear volumen raid
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 432d661d8c0ce4cae6fe08a2671e8f9d613ce351
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846786"
---
# <a name="create-volume-raid"></a>Crear volumen raid

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un RAID\-especificado de volumen 5 con tres o más discos dinámicos.  
  
> [!IMPORTANT]  
> Este comando DiskPart no está disponible en cualquier edición de Windows Vista.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|size\=<n>|La cantidad de espacio en disco, en megabytes \(MB\), que ocupará el volumen en cada disco. Si no se especifica tamaño, más grande posible RAID\-se creará el volumen 5. El disco con el menor espacio libre contiguo disponible determina el tamaño de RAID\-volumen 5 y la misma cantidad de espacio se asigna en cada disco. La cantidad de espacio en disco utilizable en RAID\-volumen 5 es menor que la cantidad combinada de espacio en disco porque parte del espacio en disco se requiere para la paridad.|  
|disco\=<n>,<n>,<n>\[,<n>,...\]|Los discos dinámicos en el que se va a crear RAID\-volumen 5. Necesitará al menos tres discos dinámicos para crear un RAID\-volumen 5. Cantidad de espacio igual a **tamaño\= <n>**  en cada disco se asigna.|  
|Alinear\=<n>|Alinea todas las extensiones de volumen para el límite de alineación más cercano. Se utiliza normalmente con el número de unidad lógica de RAID de hardware \(LUN\) matrices para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco para el límite de alineación más cercano.|  
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco automáticamente.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear un RAID\-volumen 5 de 1000 megabytes de tamaño, uso de discos 1, 2 y 3, tipo:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

