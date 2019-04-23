---
title: Se deben quitar las instantáneas de recuperación después de la conmutación por error
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4663320df91019fc7dc1d8ca7ffdb2fcc3e0de42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837686"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Se deben quitar las instantáneas de recuperación después de la conmutación por error

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*Un error a través de la máquina virtual tiene uno o más instantáneas de recuperación.*  
  
## <a name="impact"></a>**Impact**  
*Espacio disponible puede agotarse en el disco físico que almacena los archivos de instantáneas. Si esto ocurre, puede realizarse ninguna operación de disco adicional en el almacenamiento físico. Cualquier máquina virtual que se basa en el almacenamiento físico podría verse afectada. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Para cada error a través de la máquina virtual, use el cmdlet Complete-VMFailover en Windows PowerShell para quitar las instantáneas de recuperación e indicar la finalización de conmutación por error.*  
  


