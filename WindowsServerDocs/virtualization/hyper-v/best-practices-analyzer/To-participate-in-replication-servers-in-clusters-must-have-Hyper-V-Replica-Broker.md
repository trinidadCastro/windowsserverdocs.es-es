---
title: Para participar en la replicación, servidores de clústeres de conmutación por error deben tener configurado un agente de réplica de Hyper-V
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d4966396af955f9c8bad34b5b2892115e93c3b85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887976"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Para participar en la replicación, servidores de clústeres de conmutación por error deben tener configurado un agente de réplica de Hyper-V

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Para los clústeres de conmutación por error, la réplica de Hyper-V requiere el uso de un nombre de agente de réplica de Hyper-V en lugar de un nombre de servidor individuales.*  
  
## <a name="impact"></a>Impacto  
*Si la máquina virtual se mueve a un nodo de clúster de conmutación por error diferentes, la replicación no puede continuar.*  
  
## <a name="resolution"></a>Resolución  
*Use el Administrador de clústeres de conmutación por error para configurar al agente de réplica de Hyper-V. En el Administrador de Hyper-V, asegúrese de que la configuración de replicación utiliza el nombre de agente de réplica de Hyper-V como el nombre del servidor.*  
  


