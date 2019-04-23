---
title: Evitar incoherencias de alineación entre bloques virtuales y los sectores de disco físico en discos duros virtuales dinámicos o discos de diferenciación
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4973c199a5507d00e15da8f621a09f0c602a29fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833866"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Evitar incoherencias de alineación entre bloques virtuales y los sectores de disco físico en discos duros virtuales dinámicos o discos de diferenciación

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Se detectaron incoherencias de alineación para uno o varios discos duros virtuales.*  
  
### <a name="impact"></a>Impacto  
*Si los discos duros virtuales se almacenan en el disco físico que tiene un tamaño de sector de 4K, la máquina virtual o las aplicaciones que usan el disco duro virtual puede experimentar problemas de rendimiento. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Utilice al Asistente para crear un disco duro Virtual para crear un nuevo formato VHD o VHDX con formato de disco duro virtual y especifique el disco duro virtual existente como el disco de origen. El nuevo disco duro virtual se creará con la alineación entre los bloques virtuales y el disco físico.*  
  


