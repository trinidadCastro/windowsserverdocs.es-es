---
title: La resincronización de la replicación debe programarse para horas de poca actividad
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 379f8c8cd6744fe5db176efb55a84f231ce45857
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393510"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>La resincronización de la replicación debe programarse para horas de poca actividad

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*La resincronización de la replicación de las máquinas virtuales principales no está programada para horas de poca actividad.*  
  
## <a name="impact"></a>Impacto  
*Cuanto más tiempo una máquina virtual se encuentra en un estado que requiere resincronización, más tiempo aumentan los archivos de registro de replicación y se producen cambios más sin replicar en las máquinas virtuales principales. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el administrador de Hyper-V para modificar la configuración de replicación de la máquina virtual para realizar la resincronización automáticamente durante las horas de menor actividad.*  
  


