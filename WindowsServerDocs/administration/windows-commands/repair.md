---
title: Resolver
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88293422519488405d94e32596c81dbe4a697dee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371529"
---
# <a name="repair"></a>Resolver

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

repara el volumen RAID @ no__t-05 con el foco sustituyendo la región del disco con el error especificado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
| Parámetro  |                                                                                             Descripción                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disco @ no__t-0 @ no__t-1  |                                                                 Especifica el disco dinámico que reemplazará la región de disco con errores.                                                                 |
| align @ no__t-0 @ no__t-1 |          Alinea todas las extensiones de volumen o partición con el límite de alineación más cercano. *n* es el número de kilobytes \( KB @ no__t-2 desde el principio del disco hasta el límite de alineación más cercano.           |
|   Noerr    | Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |
  
## <a name="remarks"></a>Comentarios  
  
-   El disco dinámico especificado debe tener un espacio libre mayor o igual que el tamaño total de la región de disco con errores en el volumen RAID @ no__t-05.  
  
-   Se debe seleccionar un volumen en una matriz RAID @ no__t-05 para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.  
  
## <a name="BKMK_examples"></a>Example  
Para reemplazar el volumen que tiene el foco reemplazándolo por el disco dinámico 4, escriba:  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

