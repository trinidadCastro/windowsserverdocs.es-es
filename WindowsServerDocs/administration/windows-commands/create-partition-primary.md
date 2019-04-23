---
title: create partition primary
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbbb4b89540b7264fe907f79ca003117429da08e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881716"
---
# <a name="create-partition-primary"></a>create partition primary

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea una partición primaria en el disco básico con el foco.  
  
> [!CAUTION]  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|size\=<n>|Especifica el tamaño de la partición en megabytes \(MB\). Si no se especifica tamaño, la partición continuará mientras haya es espacio no asignado en la región actual.|  
|offset\=<n>|El desplazamiento en kilobytes \(KB\), que se crea la partición. Si se indica ningún desplazamiento, la partición comenzará al principio de la extensión de disco más grande que sea lo suficientemente grande como para albergarla.|  
|Alinear\=<n>|Alinea todas las extensiones de partición para el límite de alineación más cercano. Se utiliza normalmente con el número de unidad lógica de RAID de hardware \(LUN\) matrices para mejorar el rendimiento. <n> es el número de kilobytes \(KB\) desde el principio del disco para el límite de alineación más cercano.|  
|id\={ <byte> &#124; <guid> }|Especifica el tipo de partición. Este parámetro está destinado a fabricantes de equipos originales \(OEM\) uso exclusivo. Cualquier byte de tipo de partición o el GUID puede especificarse con este parámetro. DiskPart no comprueba el tipo de partición de validez, excepto para asegurarse de que es un byte en formato hexadecimal o un GUID. **Precaución:** Creación de particiones con este parámetro podría hacer que el equipo a un error o que no pueda iniciarse. A menos que sea un OEM o un profesional de TI con experiencia en discos gpt, no cree particiones en discos gpt con este parámetro. En su lugar, use siempre la **crear partición efi** comando para crear particiones de sistema EFI, el **crear partición msr** comando para crear particiones de Microsoft Reserved y el **crear partición principal** comando \(sin el **id\={byte&#124;guid}** parámetro\) para crear particiones primarias en discos gpt.<br /><br />**Discos de registro de arranque maestro**<br /><br />para el registro de arranque maestro \(MBR\) discos, especifique un byte de tipo de partición, en formato hexadecimal, para la partición. Si no se especifica este parámetro para un disco MBR, el comando crea una partición de tipo 0 x 06, que especifica que un sistema de archivos no está instalado. Algunos ejemplos son:<br /><br />-Partición de datos LDM: 0x42<br />-partición de recuperación: 0x27<br />-Partición OEM reconocido: 0x12, 0x84, 0xDE, 0xFE, 0xA0<br /><br />**Discos de tabla de particiones GUID**<br /><br />para la tabla de particiones GUID \(gpt\) discos, puede especificar un GUID de tipo de partición para la partición que desea crear. GUID reconocidos incluyen:<br /><br />-Partición de sistema EFI: c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Partición de Microsoft Reserved: e3c9e316\-0b5c\-4db8\-817D\-f92df00215ae<br />-Partición de datos básica: ebd0a0a2\-b9e5\-4433\-87c 0\-68b6b72699c7<br />-Partición de metadatos LDM en un disco dinámico: 5808c8aa\-7e8f\-42e0\-85d2\-e1e90434cfb3<br />-Partición de datos LDM en un disco dinámico: af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-partición de recuperación: de94bba4\-d. 06 1\-4D 40\-a16a\-bfd50179d6ac<br /><br />Si no se especifica este parámetro para un disco gpt, el comando crea una partición de datos básica.|  
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin el parámetro noerr, un error provoca que DiskPart se cierre con un código de error.|  
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear la partición, ésta recibe el foco automáticamente.  
  
-   La partición no recibe una letra de unidad. Debe usar el **asignar** comando DiskPart para asignar una letra de unidad a la partición.  
  
-   Debe seleccionarse un disco básico para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco básico y desplace el foco a ella.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear una partición primaria de 1000 megabytes de tamaño, escriba:  
  
```  
create partition primary size=1000  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

