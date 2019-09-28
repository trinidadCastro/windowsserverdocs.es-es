---
title: Configuración de sistemas operativos invitados para las copias de seguridad basadas en VSS para habilitar instantáneas coherentes con la aplicación para la réplica de Hyper-V
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 032ca585da1c556fff6f9e06b3bde0662a5d64db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364962"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configuración de sistemas operativos invitados para las copias de seguridad basadas en VSS para habilitar instantáneas coherentes con la aplicación para la réplica de Hyper-V

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
*Las instantáneas coherentes con la aplicación requieren que los servicios de instantáneas de volumen (VSS) estén habilitados y configurados en los sistemas operativos invitados de las máquinas virtuales que participan en la replicación.*  
  
## <a name="impact"></a>Impacto  
*Even si se especifican instantáneas coherentes con la aplicación en la configuración de replicación, Hyper-V no las usará a menos que se configure VSS. Esto afecta a las siguientes máquinas virtuales:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use conexión a máquina virtual para instalar los servicios de integración en la máquina virtual.*  
  


