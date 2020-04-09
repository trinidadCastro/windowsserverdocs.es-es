---
title: resolver
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46b98938394c10e31d4999ff0e060e10f7da9bdc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835938"
---
# <a name="repair"></a>resolver

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

repara el volumen RAID\-5 con el foco sustituyendo la región de disco con el error en el disco dinámico especificado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
| Parámetro  |                                                                                             Descripción                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <n> de\=de disco  |                                                                 Especifica el disco dinámico que reemplazará la región de disco con errores.                                                                 |
| alinear\=<n> |          Alinea todas las extensiones de volumen o partición con el límite de alineación más cercano. *n* es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano.           |
|   noerr    | solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |
  
## <a name="remarks"></a>Comentarios  
  
-   El disco dinámico especificado debe tener un espacio libre mayor o igual que el tamaño total de la región de disco con errores en el volumen RAID\-5.  
  
-   Se debe seleccionar un volumen en una matriz RAID\-5 para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para reemplazar el volumen que tiene el foco reemplazándolo por el disco dinámico 4, escriba:  
  
```  
repair disk=4  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

