---
title: Reparación
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1e4b9cde10e11558aaa95edda94921144dac1f86
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441804"
---
# <a name="repair"></a>Reparación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Repara el RAID\-volumen 5 tiene el foco mediante la sustitución de la región del disco con errores con el disco dinámico especificado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
| Parámetro  |                                                                                             Descripción                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disk\=<n>  |                                                                 Especifica el disco dinámico que reemplazará a la región del disco con errores.                                                                 |
| Alinear\=<n> |          Alinea todas las extensiones de volumen o partición para el límite de alineación más cercano. *n* es el número de kilobytes \(KB\) desde el principio del disco para el límite de alineación más cercano.           |
|   noerr    | sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error. |
  
## <a name="remarks"></a>Comentarios  
  
-   El disco dinámico especificado debe tener espacio mayor o igual que el tamaño total de la región del disco con errores en el RAID\-volumen 5.  
  
-   Un volumen de RAID\-matriz 5 debe seleccionarse para que esta operación se realice correctamente. Use la **seleccione volumen** comando para seleccionar un volumen y desplace el foco a ella.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para reemplazar el volumen con el foco al sustituirla por disco dinámico 4, escriba:  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

