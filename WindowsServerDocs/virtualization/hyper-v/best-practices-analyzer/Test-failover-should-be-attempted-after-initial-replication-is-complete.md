---
title: La conmutación por error de prueba debe intentarse una vez completada la replicación inicial
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec3ad3994227eb14d1d2e53842c755af76ac538d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364700"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>La conmutación por error de prueba debe intentarse una vez completada la replicación inicial

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="problem"></a>Problema  
*No se ha realizado ninguna conmutación por error de prueba en al menos un mes.*  
  
## <a name="impact"></a>Impacto  
*There es la confirmación de que una conmutación por error planeada o no planeada se realizará correctamente o las operaciones de carga de trabajo continuarán correctamente después de una conmutación por error. Esto afecta a las siguientes máquinas virtuales:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el administrador de Hyper-V para realizar una conmutación por error de prueba.*  
  


