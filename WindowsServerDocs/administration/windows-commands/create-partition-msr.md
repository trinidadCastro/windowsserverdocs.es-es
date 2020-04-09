---
title: create partition msr
description: Tema de comandos de Windows para Create Partition MSR, que crea una partición reservada de Microsoft (MSR) en un disco de tabla de particiones GUID (GPT).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0f0390cd3b9f390e1f65b034fecd00d8ff41079
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847018"
---
# <a name="create-partition-msr"></a>create partition msr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partición reservada de Microsoft (MSR) en un disco de tabla de particiones GUID (GPT).
  
> [!CAUTION]  
> Tenga mucho cuidado al usar este comando. Dado que los discos GPT requieren un diseño de partición específico, la creación de particiones reservadas de Microsoft podría provocar que el disco no se pudiera leer.
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                         Descripción                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamaño\=<n>  |               Tamaño de la partición en megabytes \(MB\). La partición es, como mínimo, en bytes como el número especificado por <n>. Si no se proporciona ningún tamaño, la partición continúa hasta que no haya más espacio libre en la región actual.               |
| desplazamiento\=<n> | Especifica el desplazamiento en kilobytes \(KB\), en el que se crea la partición. El desplazamiento se redondea hacia arriba para llenar completamente el tamaño del sector que se utiliza. Si no se indica un desplazamiento, la partición se colocará en la primera zona del disco que sea lo suficientemente grande como para albergarla. |
|    noerr    |                            solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                             |
  
## <a name="remarks"></a>Comentarios  
  
-   En los discos GPT que se utilizan para arrancar el sistema operativo Windows, el Extensible Firmware Interface \(EFI\) partición del sistema es la primera partición del disco, seguida de la partición reservada de Microsoft. los discos GPT que se usan únicamente para el almacenamiento de datos no tienen una partición del sistema EFI, en cuyo caso la partición reservada de Microsoft es la primera partición.  
  
-   Windows no monta las particiones reservadas de Microsoft. No se puede almacenar datos en ellas y no se pueden eliminar.  
  
-   Se requiere una partición reservada de Microsoft en cada disco GPT. El tamaño de esta partición depende del tamaño total del disco GPT. El tamaño del disco GPT debe ser de al menos 32 MB para crear una partición reservada de Microsoft.  
  
-   Se debe seleccionar un disco GPT básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco GPT básico y desplazar el foco a él.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para crear una partición reservada de Microsoft de 1000 megabytes de tamaño, escriba:  
  
```  
create partition msr size=1000  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

