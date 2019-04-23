---
title: Volumen de atributos
description: Tema de los comandos de Windows para **atributos volumen** -muestra, Establece o borra los atributos de un volumen.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37af55ee2a041fbcf8068e0def72147732d3a687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846586"
---
# <a name="attributes-volume"></a>Volumen de atributos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra, Establece o borra los atributos de un volumen.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|set|Establece el atributo especificado del volumen que tiene el foco.|  
|clear|Borra el atributo especificado del volumen que tiene el foco.|  
|ReadOnly|Especifica que el volumen es de lectura\-solo.|  
|hidden|Especifica que el volumen está oculto.|  
|NODEFAULTDRIVELETTER|Especifica que el volumen no recibe una letra de unidad de forma predeterminada.|  
|shadowcopy|Especifica que el volumen es un volumen de instantáneas.|  
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   En el registro de arranque maestro \(MBR\) discos, el **oculto**, **readonly**, y **nodefaultdriveletter** parámetros se aplican a todos los volúmenes en el disco.  
  
-   En la tabla de particiones GUID básica \(gpt\) discos y en dinámico discos MBR y gpt, el **oculto**, **readonly**, y **nodefaultdriveletter** parámetros que se aplican solo al volumen seleccionado.  
  
-   Debe seleccionarse un volumen para el **atributos volumen** comando se ejecute correctamente. Use la **seleccione volumen** comando para seleccionar un volumen y desplace el foco a ella.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para mostrar los atributos actuales en el volumen seleccionado, escriba:  
  
```  
attributes volume  
```  
  
Para establecer el volumen seleccionado como oculto y lectura\-solo, escriba:  
  
```  
attributes volume set hidden readonly  
```  
  
Para quitar los ocultos y lectura\-sólo los atributos en el volumen seleccionado, escriba:  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

