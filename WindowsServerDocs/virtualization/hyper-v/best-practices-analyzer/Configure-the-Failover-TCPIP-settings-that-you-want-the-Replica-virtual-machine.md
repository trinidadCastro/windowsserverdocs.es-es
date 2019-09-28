---
title: Configure las opciones de TCP/IP de conmutación por error que desea que use la máquina virtual de réplica en caso de una conmutación por error.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3f2681694d87b34369b29be6216ebec9210c6024
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366294"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Configure las opciones de TCP/IP de conmutación por error que desea que use la máquina virtual de réplica en caso de una conmutación por error.

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
*Las máquinas virtuales de réplica configuradas con una dirección IP estática deben configurarse para usar una dirección IP diferente de su equivalente de máquina virtual principal en caso de conmutación por error.*  
  
## <a name="impact"></a>Impacto  
es posible que @no__t 0Clients con la carga de trabajo compatible con la máquina virtual principal no pueda conectarse a la máquina virtual de réplica después de una conmutación por error. Además, la dirección IP original de la máquina virtual principal no será válida en la topología de red de la máquina virtual de réplica. Esto afecta a las siguientes máquinas virtuales: *  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el administrador de Hyper-V para configurar la dirección IP que la máquina virtual de réplica debe usar en caso de conmutación por error.*  
  


