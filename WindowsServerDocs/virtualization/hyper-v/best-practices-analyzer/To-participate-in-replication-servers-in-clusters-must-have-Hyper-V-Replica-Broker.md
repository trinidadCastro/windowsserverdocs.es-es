---
title: Para participar en la replicación, los servidores de los clústeres de conmutación por error deben tener configurado un agente de réplicas de Hyper-V
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2e15d2c4a467807397ef4712d2df1730b40d8024
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364597"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Para participar en la replicación, los servidores de los clústeres de conmutación por error deben tener configurado un agente de réplicas de Hyper-V

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*En el caso de los clústeres de conmutación por error, réplica de Hyper-V requiere el uso de un nombre de agente de réplicas de Hyper-V en lugar de un nombre de servidor individual.*  
  
## <a name="impact"></a>Impacto  
*Si la máquina virtual se mueve a otro nodo de clúster de conmutación por error, la replicación no puede continuar.*  
  
## <a name="resolution"></a>Resolución  
*Use Administrador de clústeres de conmutación por error para configurar el agente de réplicas de Hyper-V. En el administrador de Hyper-V, asegúrese de que la configuración de replicación usa el nombre del agente de réplicas de Hyper-V como nombre del servidor.*  
  


