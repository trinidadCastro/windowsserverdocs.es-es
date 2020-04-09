---
title: Configurar una directiva para limitar el tráfico de replicación en la red
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2c358cd930f2b95412b40aa6c87b0bf9ebb5b741
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862178"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurar una directiva para limitar el tráfico de replicación en la red

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Es posible que no haya un límite en la cantidad de ancho de banda de red que puede consumir la replicación.*  
  
## <a name="impact"></a>Impacto  
*El ancho de banda de red podría estar completamente dominado por el tráfico de replicación, lo que afecta a otras actividades de red críticas. Esto afecta a los siguientes puertos:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Si usa otro método para limitar el tráfico de red, puede pasarlo por alto. De lo contrario, use el editor de directiva de grupo para configurar una directiva que limitará el tráfico de red al puerto pertinente del servidor réplica.*  
  
  


