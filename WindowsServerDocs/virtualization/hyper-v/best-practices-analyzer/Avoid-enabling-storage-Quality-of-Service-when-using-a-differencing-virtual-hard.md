---
title: Evitar tener que habilitar la calidad de servicio de almacenamiento cuando se usa un disco duro virtual de diferenciación cuando los discos duros virtuales primarios y secundarios se encuentran en volúmenes diferentes
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2bdc8462c4d9dc50dbb69792f2f294add0ca3a74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856206"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evitar tener que habilitar la calidad de servicio de almacenamiento cuando se usa un disco duro virtual de diferenciación cuando los discos duros virtuales primarios y secundarios se encuentran en volúmenes diferentes

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.
  
## <a name="issue"></a>**Problema**  
*Un disco duro virtual de diferenciación con los discos duros virtuales primarios y secundarios en volúmenes diferentes tiene habilitada de calidad de servicio de almacenamiento.*  
  
## <a name="impact"></a>**Impact**  
*Esta configuración puede producir almacenamiento inesperado de comportamiento de calidad de servicio para el disco duro virtual de diferenciación, así como otros discos duros virtuales en los volúmenes primarios y secundarios. Esto afecta a la siguiente los discos duros virtuales:*  
  
\<lista de discos duros virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Deshabilitar la calidad de servicio de almacenamiento en los discos duros virtuales que se hace referencia, o realizar una migración de almacenamiento para mover el elemento primario y el disco duro virtual de secundario en el mismo volumen.*  
  


