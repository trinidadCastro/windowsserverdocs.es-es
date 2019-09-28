---
title: Configurar controladores SCSI solo cuando sea compatible con el sistema operativo invitado
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: da8d929a8f06f58610913d28d2f1e90299efb235
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366416"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurar controladores SCSI solo cuando sea compatible con el sistema operativo invitado

>Se aplica a: Windows Server 2016


  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Una máquina virtual está configurada con una controladora SCSI que no se puede usar porque el sistema operativo invitado no es compatible con controladores SCSI.*  
  
## <a name="impact"></a>Impacto  
  
las máquinas @no__t 0Virtual no pueden usar el almacenamiento conectado a la controladora SCSI. Esto afecta a las siguientes máquinas virtuales: *  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
  
@no__t 0Shut la máquina virtual y use el administrador de Hyper-V para quitar la controladora SCSI de la máquina virtual. A continuación, reinicie la máquina virtual. *  
  


