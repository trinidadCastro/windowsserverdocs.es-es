---
title: crear partición EFI
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76d97129fd67345f23eee2fc7b300493a1632cc6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379008"
---
# <a name="create-partition-efi"></a>crear partición EFI

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En los equipos Itanium @ no__t-0based, crea una partición del sistema Extensible Firmware Interface \(EFI @ no__t-2 en una tabla de particiones GUID \(gpt @ no__t-4 Disk.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                             Descripción                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Size @ no__t-0 @ no__t-1  |                         El tamaño de la partición en megabytes \(MB @ no__t-1. Si no se proporciona ningún tamaño, la partición continúa hasta que no haya más espacio libre en la región actual.                         |
| offset @ no__t-0 @ no__t-1 |             Desplazamiento en kilobytes \(KB @ no__t-1, en el que se crea la partición. Si no se proporciona ningún desplazamiento, la partición se coloca en la primera extensión del disco que sea lo suficientemente grande como para almacenarla.              |
|    Noerr    | Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |
  
## <a name="remarks"></a>Comentarios  
  
-   Una vez creada la partición, se asigna el foco a la nueva partición.  
  
-   Se debe seleccionar un disco GPT para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.  
  
## <a name="BKMK_examples"></a>Example  
Para crear una partición EFI de 1000 megabytes en el disco seleccionado, escriba:  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

