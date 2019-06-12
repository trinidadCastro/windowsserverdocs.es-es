---
title: crear la partición msr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fa9ba46418c3ed3b7999a734b4c0df40dce5027
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434177"
---
# <a name="create-partition-msr"></a>crear la partición msr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un Reserved Microsoft \(MSR\) partición en una tabla de particiones GUID \(gpt\) disco.  
  
> [!CAUTION]  
> Tenga mucho cuidado al utilizar este comando. Dado que los discos gpt requieren un diseño de partición específica, la creación de particiones de Microsoft Reserved podría provocar que el disco no sea legible.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                         Descripción                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |               El tamaño de la partición en megabytes \(MB\). La partición es al menos tan larga en bytes, como el número especificado por <n>. Si no se especifica tamaño, la partición continuará mientras haya espacio libre en la región actual.               |
| offset\=<n> | Especifica el desplazamiento en kilobytes \(KB\), que se crea la partición. El desplazamiento se redondea hacia arriba para llenar completamente el tamaño en sectores se utiliza. Si se indica ningún desplazamiento, la partición se coloca en la primera zona del disco que sea lo suficientemente grande como para albergarla. |
|    noerr    |                            sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.                             |
  
## <a name="remarks"></a>Comentarios  
  
-   En discos gpt que se utilizan para arrancar el sistema operativo Windows, la interfaz de Firmware Extensible \(EFI\) partición del sistema es la primera partición del disco, seguida de la partición Reserved Microsoft. los discos GPT que se usan únicamente para el almacenamiento de datos no tiene una partición de sistema EFI, en cuyo caso la partición de Microsoft Reserved es la primera partición.  
  
-   Windows no montan particiones de Microsoft Reserved. No se puede almacenar datos en ellas y no puede eliminarlas.  
  
-   Se requiere una partición de Microsoft Reserved en todos los discos gpt. El tamaño de esta partición depende del tamaño total del disco gpt. El tamaño del disco gpt debe ser al menos 32 MB para crear una partición Reserved Microsoft.  
  
-   Debe seleccionarse un disco gpt básico para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco gpt básico y desplace el foco a ella.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear una partición de Microsoft Reserved de 1000 megabytes de tamaño, escriba:  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

