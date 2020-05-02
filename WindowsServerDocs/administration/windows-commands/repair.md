---
title: reparación
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89c09608e64b408db9c7c79269046195e005c187
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722389"
---
# <a name="repair"></a>reparación

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

repara el volumen\-RAID 5 con el foco reemplazando la región del disco con el error especificado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
| Parámetro  |                                                                                             Descripción                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| discos\=<n>  |                                                                 Especifica el disco dinámico que reemplazará la región de disco con errores.                                                                 |
| alinea\=<n> |          Alinea todas las extensiones de volumen o partición con el límite de alineación más cercano. *n* es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano.           |
|   noerr    | solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |
  
## <a name="remarks"></a>Observaciones  
  
-   El disco dinámico especificado debe tener un espacio libre mayor o igual que el tamaño total de la región de disco con errores en\-el volumen RAID 5.  
  
-   Se debe seleccionar un volumen\-en una matriz RAID 5 para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.  
  
## <a name="examples"></a>Ejemplos  
Para reemplazar el volumen que tiene el foco reemplazándolo por el disco dinámico 4, escriba:  
  
```  
repair disk=4  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

