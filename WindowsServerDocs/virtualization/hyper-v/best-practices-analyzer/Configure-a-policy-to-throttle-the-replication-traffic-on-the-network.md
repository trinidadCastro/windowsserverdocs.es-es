---
title: Configurar una directiva para limitar el tráfico de replicación en la red
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2c1f1865fa1d611c0b5baaf981140f9807b51458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818696"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurar una directiva para limitar el tráfico de replicación en la red

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
*No puede haber un límite en la cantidad de ancho de banda de red que la replicación puede consumir.*  
  
## <a name="impact"></a>Impacto  
*Ancho de banda de red podría convertirse en dominado por el tráfico de replicación, que afectan a otras actividades de red críticos. Esto afecta a los puertos siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Si usa otro método para limitar el tráfico de red, puede omitir esto. En caso contrario, utilice el Editor de directivas de grupo para configurar una directiva que limita el tráfico de red al puerto correspondiente del servidor réplica.*  
  
  


