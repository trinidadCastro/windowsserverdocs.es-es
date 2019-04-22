---
title: creación de reflejo de volumen
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c66f21f55201d9d784b1ab0d7b729bc272589e5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822976"
---
# <a name="create-volume-mirror"></a>creación de reflejo de volumen

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un reflejo del volumen mediante el uso de los dos discos dinámicos especificados.  
  
> [!NOTE]  
> Este comando solo está disponible en Windows 7 y Windows Server 2008 R2.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|size\=<n>|Especifica la cantidad de espacio en disco, en megabytes \(MB\), que ocupará el volumen en cada disco. Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio libre restante en el disco más pequeño y cantidades equivalentes de espacio en los discos sucesivos.|  
|disco\=<n>,<n>\[,<n>,...\]|Especifica los discos dinámicos en el que se crea el volumen reflejado. Se necesitan dos discos dinámicos para crear un volumen reflejado. Cantidad de espacio que es igual al tamaño especificado con el **tamaño** en cada disco se asigna el parámetro.|  
|Alinear\=<n>|Alinea todas las extensiones de volumen para el límite de alineación más cercano. Este parámetro se usa normalmente con el número de unidad lógica de RAID de hardware \(LUN\) matrices para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco para el límite de alineación más cercano.|  
|noerr|Se utiliza sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco automáticamente.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear un volumen reflejado de 1000 megabytes de tamaño, en los discos 1 y 2, escriba:  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

