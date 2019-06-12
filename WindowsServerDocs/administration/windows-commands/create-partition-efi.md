---
title: Crear partición efi
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 99970fba41a747a6bb4b1ca6cc4b7f603c547790
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434161"
---
# <a name="create-partition-efi"></a>Crear partición efi

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Itanium\-los equipos basados en, se crea una interfaz de Firmware Extensible \(EFI\) partición del sistema en una tabla de particiones GUID \(gpt\) disco.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                             Descripción                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                         El tamaño de la partición en megabytes \(MB\). Si no se especifica tamaño, la partición continuará mientras haya espacio libre en la región actual.                         |
| offset\=<n> |             El desplazamiento en kilobytes \(KB\), que se crea la partición. Si se indica ningún desplazamiento, la partición se coloca en la primera zona del disco que sea lo suficientemente grande como para albergarla.              |
|    noerr    | sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error. |
  
## <a name="remarks"></a>Comentarios  
  
-   Una vez creada la partición, ésta recibe el foco a la nueva partición.  
  
-   Debe seleccionarse un disco gpt para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear una partición EFI de 1000 megabytes en el disco seleccionado, escriba:  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

