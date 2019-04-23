---
title: Asegúrese de que hay suficiente espacio de disco físico cuando las máquinas virtuales usan discos duros virtuales de diferenciación
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6d4d219402cdc321a75cc27d75ea7749eb6127e0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855576"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Asegúrese de que hay suficiente espacio de disco físico cuando las máquinas virtuales usan discos duros virtuales de diferenciación

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
*Uno o más máquinas virtuales usan discos duros virtuales de diferenciación.*  
  
## <a name="impact"></a>Impacto  
*Discos duros virtuales de diferenciación requieren espacio disponible en el volumen de host para que se puede asignar espacio cuando se producen operaciones de escritura en los discos duros virtuales. Si se agota el espacio disponible, podría verse afectada cualquier máquina virtual que se basa en el almacenamiento físico. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Supervisar espacio en disco disponible para asegurarse de que hay suficiente espacio para la expansión del disco duro virtual. Considere la posibilidad de combinar discos duros virtuales de diferenciación en su elemento primario. En el Administrador de Hyper-V, inspeccione el disco de diferenciación para determinar el disco duro virtual primario. Si combina un disco de diferenciación en un disco primario que comparte otros discos de diferenciación, esa acción dañará la relación entre los discos de diferencias y el disco primario, haciéndolos inutilizable. Después de comprobar que el disco duro virtual principal no está compartido, puede usar al Asistente para editar discos para combinar el disco de diferenciación en el disco duro virtual primario.*  
  


