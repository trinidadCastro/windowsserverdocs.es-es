---
title: Crear volumen simple
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d754a6b5788656132459a0b4a42954e9084f26bb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836506"
---
# <a name="create-volume-simple"></a>Crear volumen simple

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un volumen simple en el disco dinámico especificado.  
  
> [!IMPORTANT]  
> para Windows Vista, este comando DiskPart sólo está disponible en las ediciones de Windows Vista Ultimate, Windows Vista Enterprise y Windows Vista Business.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|size\=<n>|El tamaño del volumen en megabytes \(MB\). Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio libre restante en el disco.|  
|disk\=<n>|El disco dinámico en el que se crea el volumen. Si no se especifica ningún disco, se usa el disco actual.|  
|Alinear\=<n>|Alinea todas las extensiones de volumen para el límite de alineación más cercano. Se utiliza normalmente con el número de unidad lógica de RAID de hardware \(LUN\) matrices para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco para el límite de alineación más cercano.|  
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco automáticamente.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear un volumen de 1000 megabytes de tamaño, en el disco 1, escriba:  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

