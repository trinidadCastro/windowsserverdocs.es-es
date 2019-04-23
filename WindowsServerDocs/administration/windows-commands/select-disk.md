---
title: select disk
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e8352b45cf3a46f14828c9c6796e4ec73499d5a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858966"
---
# <a name="select-disk"></a>select disk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

selecciona el disco especificado y cambia el foco a ella.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> El **<disk path>**, **sistema**, y **siguiente** parámetros solo están disponibles en Windows 7 y Windows Server 2008 R2.  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|<n>|Especifica el número del disco que se va a recibir el foco. Puede ver los números para todos los discos en el equipo mediante el **disco lista** comando DiskPart. **Nota:** Al configurar los sistemas con varios discos, no use **seleccione disco\=0** para especificar el disco del sistema. El equipo puede volver a asignar números de disco cuando se reinicia y los diferentes equipos con la misma configuración de disco pueden tener números de disco diferente.|  
|<disk path>|Especifica la ubicación del disco para recibir el foco, por ejemplo, **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . Para ver la ruta de acceso de ubicación de un disco, selecciónelo y, a continuación, escriba **disco detalle**.|  
|Sistema|En equipos con BIOS, especifica que el disco 0 recibe el foco. En los equipos EFI, el disco que contiene la partición del sistema EFI \(ESP\) que se usa para el arranque actual recibe el foco. En los equipos EFI, el comando generará un error si no hay ningún ESP, si hay más de una ESP o el equipo se arranca desde el entorno de preinstalación de Windows \(Windows PE\).|  
|siguiente|Una vez que se ha seleccionado un disco, este comando recorre en iteración todos los discos en la lista de discos. Al ejecutar este comando, el disco en la lista siguiente recibirá el foco.|  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para cambiar el foco en el disco 1, escriba:  
  
```  
select disk=1  
```  
  
Para seleccionar un disco mediante el uso de su ruta de acceso de ubicación, escriba:  
  
```  
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)  
```  
  
Para cambiar el foco en el disco del sistema, escriba:  
  
```  
select disk=system  
```  
  
Para cambiar el foco en el siguiente disco en el equipo, escriba:  
  
```  
select disk=next  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

