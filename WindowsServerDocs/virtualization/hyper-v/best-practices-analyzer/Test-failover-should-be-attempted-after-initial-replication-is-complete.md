---
title: Conmutación por error de prueba debe realizarse una vez completada la replicación inicial
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 347b089330548e5fb2631e43ca66c96bb6bc4e9d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880416"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>Conmutación por error de prueba debe realizarse una vez completada la replicación inicial

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="problem"></a>Problema  
*No ha habido ninguna conmutación por error de prueba en al menos un mes.*  
  
## <a name="impact"></a>Impacto  
*No hay ninguna confirmación de que se realizará correctamente una conmutación por error planeada o no planeada o las operaciones de carga de trabajo continúen funcionando correctamente después de una conmutación por error. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el Administrador de Hyper-V para llevar a cabo una conmutación por error de prueba.*  
  


