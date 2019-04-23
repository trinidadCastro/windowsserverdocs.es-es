---
title: extend
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84047c690006bf727bc12855576960bbf67d1617
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857936"
---
# <a name="extend"></a>extend

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

extiende el volumen o partición que tiene el foco y su sistema de archivos a libre \(sin asignar\) espacio en un disco.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|size\=<n>|Especifica la cantidad de espacio en megabytes \(MB\) para agregar a la partición o volumen actual. Si no se especifica tamaño, se usa todo el espacio libre contiguo que está disponible en el disco.|  
|disk\=<n>|Especifica el disco en el que se extiende el volumen o partición. Si no se especifica ningún disco, se extiende el volumen o partición del disco actual.|  
|sistema de archivos|amplía el sistema de archivos del volumen que tiene el foco. Para su uso en los discos donde el sistema de archivos no se ha ampliado con el volumen.|  
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   En discos básicos, debe ser el espacio libre en el mismo disco que el volumen o partición tiene el foco. Debe seguir inmediatamente el volumen o partición tiene el foco \(es decir, debe comenzar en el siguiente desplazamiento sector\).  
  
-   En discos dinámicos con volúmenes simples o distribuidos, un volumen puede extenderse hacia cualquier espacio libre de cualquier disco dinámico. Con este comando, puede convertir un volumen dinámico simple en un volumen dinámico distribuido. Reflejados, RAID\-no se pueden ampliar volúmenes seccionados y 5.  
  
-   Si se formateó la partición anteriormente con el sistema de archivos NTFS, el sistema de archivos se extiende automáticamente para llenar la partición de mayor tamaño y se producirá ninguna pérdida de datos.  
  
-   Si se formateó la partición anteriormente con un sistema de archivos distinto de NTFS, el comando produce un error sin cambiar la partición.  
  
-   Si anteriormente no se formateó la partición con un sistema de archivos, la partición se extenderá.  
  
-   La partición debe tener un volumen asociado antes de que se puede ampliar.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para extender el volumen o partición tiene el foco, 500 megabytes, en el disco 3, escriba:  
  
```  
extend size=500 disk=3  
```  
  
Para extender el sistema de archivos de un volumen una vez que se ha ampliado, escriba:  
  
```  
extend filesystem  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

