---
title: Configurar controladores de SCSI solo si es compatible con el sistema operativo invitado
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3dc48602ab6c71c60fdb734ca98cf1359f58d87c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830396"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurar controladores de SCSI solo si es compatible con el sistema operativo invitado

>Se aplica a: Windows Server 2016


  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Una máquina virtual se configura con una controladora SCSI que no se puede usar porque el sistema operativo invitado no es compatible con controladoras SCSI.*  
  
## <a name="impact"></a>Impacto  
  
*Las máquinas virtuales no se puede usar el almacenamiento conectado a la controladora SCSI. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
  
*Apague la máquina virtual y utilice el Administrador de Hyper-V para quitar la controladora SCSI de la máquina virtual. A continuación, reinicie la máquina virtual.*  
  


