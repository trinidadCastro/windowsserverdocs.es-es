---
title: Configuración de sistemas operativos invitados para las copias de seguridad basadas en VSS para habilitar instantáneas coherentes con la aplicación para la réplica de Hyper-V
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f7a77b2cb743f478525f839e1c64ecc892b3fb04
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862068"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configuración de sistemas operativos invitados para las copias de seguridad basadas en VSS para habilitar instantáneas coherentes con la aplicación para la réplica de Hyper-V

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Las instantáneas coherentes con la aplicación requieren que los servicios de instantáneas de volumen (VSS) estén habilitados y configurados en los sistemas operativos invitados de las máquinas virtuales que participan en la replicación.*  
  
## <a name="impact"></a>Impacto  
*Incluso si las instantáneas coherentes con la aplicación se especifican en la configuración de replicación, Hyper-V no las usará a menos que se configure VSS. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use conexión a máquina virtual para instalar los servicios de integración en la máquina virtual.*  
  


