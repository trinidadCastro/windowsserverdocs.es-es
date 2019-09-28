---
title: volumen de atributos
description: 'Windows Commands topic for **attributes Volume** : muestra, establece o borra los atributos de un volumen.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 225a10307123763d1a024fcc08fbae536fd0b5df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382583"
---
# <a name="attributes-volume"></a>volumen de atributos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra, establece o borra los atributos de un volumen.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|set|Establece el atributo especificado del volumen que tiene el foco.|  
|clear|Borra el atributo especificado del volumen que tiene el foco.|  
|ReadOnly|Especifica que el volumen es Read @ no__t-0only.|  
|plusvalía|Especifica que el volumen está oculto.|  
|nodefaultdriveletter|Especifica que el volumen no recibe una letra de unidad de forma predeterminada.|  
|ShadowCopy|Especifica que el volumen es un volumen de instantánea.|  
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   En los discos básicos de registro de arranque maestro \(MBR @ no__t-1, los parámetros **Hidden**, **ReadOnly**y **nodefaultdriveletter** se aplican a todos los volúmenes del disco.  
  
-   En la tabla de particiones GUID básica \(gpt @ no__t-1 Disks y en discos MBR y GPT dinámicos, los parámetros **Hidden**, **ReadOnly**y **nodefaultdriveletter** solo se aplican al volumen seleccionado.  
  
-   Se debe seleccionar un volumen para que el comando de **volumen de atributos** se ejecute correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.  
  
## <a name="BKMK_examples"></a>Example  
Para mostrar los atributos actuales en el volumen seleccionado, escriba:  
  
```  
attributes volume  
```  
  
Para establecer el volumen seleccionado como oculto y leer @ no__t-0only, escriba:  
  
```  
attributes volume set hidden readonly  
```  
  
Para quitar los atributos Hidden y Read @ no__t-0only en el volumen seleccionado, escriba:  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

