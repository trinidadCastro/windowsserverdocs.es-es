---
title: create partition efi
description: Tema de comandos de Windows para Create Partition EFI, que crea una partición del sistema Extensible Firmware Interface (EFI) en un disco de tabla de particiones GUID (GPT) en equipos basados en Itanium.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c61f103fc719c8144e942f64172e3be554a414
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847048"
---
# <a name="create-partition-efi"></a>create partition efi

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En equipos basados en Itanium, crea una partición del sistema Extensible Firmware Interface (EFI) en un disco de tabla de particiones GUID (GPT).

## <a name="syntax"></a>Sintaxis  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                             Descripción                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamaño\=<n>  |                         Tamaño de la partición en megabytes \(MB\). Si no se proporciona ningún tamaño, la partición continúa hasta que no haya más espacio libre en la región actual.                         |
| desplazamiento\=<n> |             Desplazamiento en kilobytes \(KB\), en el que se crea la partición. Si no se indica un desplazamiento, la partición se colocará en la primera zona del disco que sea lo suficientemente grande como para albergarla.              |
|    noerr    | solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear la partición, ésta recibe el foco.  
  
-   Se debe seleccionar un disco GPT para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para crear una partición EFI de 1000 megabytes en el disco seleccionado, escriba:  
  
```  
create partition efi size=1000  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

