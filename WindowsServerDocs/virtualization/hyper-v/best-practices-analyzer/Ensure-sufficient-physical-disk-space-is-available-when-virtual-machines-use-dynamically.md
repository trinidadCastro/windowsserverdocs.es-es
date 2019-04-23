---
title: Asegúrese de que hay suficiente espacio de disco físico cuando las máquinas virtuales usen discos duros virtuales de expansión dinámica
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 09e481b99594ac543dadab2b60bf9b3f4c29e54b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883296"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Asegúrese de que hay suficiente espacio de disco físico cuando las máquinas virtuales usen discos duros virtuales de expansión dinámica

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
*Uno o más máquinas virtuales usan discos duros virtuales de expansión dinámica.*  
  
## <a name="impact"></a>Impacto  
*Expansión dinámica discos duros virtuales requieren espacio disponible en el volumen de host para que se puede asignar espacio cuando se producen operaciones de escritura en los discos duros virtuales. Si se agota el espacio disponible, podría verse afectada cualquier máquina virtual que se basa en el almacenamiento físico. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Supervisar espacio en disco disponible para asegurar suficiente espacio está disponible para la expansión. Considere la posibilidad de apagar la máquina virtual y use el Asistente para editar discos en el Administrador de Hyper-V para convertir cada disco duro virtual expansión dinámica para esta máquina virtual en un disco duro virtual con tamaño fijo.*  
  


