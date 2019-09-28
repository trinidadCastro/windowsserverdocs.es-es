---
title: Configurar una directiva para limitar el tráfico de replicación en la red
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5b3afd594f56973007a2f0f4318de8a8c7a98209
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365115"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurar una directiva para limitar el tráfico de replicación en la red

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Es posible que no haya un límite en la cantidad de ancho de banda de red que puede consumir la replicación.*  
  
## <a name="impact"></a>Impacto  
el ancho de banda de *Network podría estar completamente dominado por el tráfico de replicación, lo que afecta a otras actividades de red críticas. Esto afecta a los siguientes puertos:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*If usa otro método para limitar el tráfico de red, puede pasarlo por alto. De lo contrario, use el editor de directiva de grupo para configurar una directiva que limitará el tráfico de red al puerto correspondiente del servidor réplica.*  
  
  


