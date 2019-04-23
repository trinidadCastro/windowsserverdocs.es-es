---
title: La replicación está en pausa para una o más máquinas virtuales en este servidor
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 248b5fbdbfb54380e441d14cde6beaa9146ce800
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827776"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La replicación está en pausa para una o más máquinas virtuales en este servidor

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*La replicación está en pausa para una o varias de las máquinas virtuales. Mientras la máquina virtual principal está en pausa, los cambios que se producen se agrupan y se enviarán a la máquina virtual de réplica una vez que se reanuda la replicación.*  
  
## <a name="impact"></a>Impacto  
*Mientras está en pausa la replicación, los cambios acumulados que se producen en la máquina virtual principal consumirá espacio en disco disponible en el servidor principal. Después de que se reanuda la replicación, puede haber un grandes ráfagas de tráfico de red al servidor réplica. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Confirme que se pretendía que va a pausar la replicación. Si se pausó la replicación para la conectividad de red o de espacio en disco baja dirección, reanudar la replicación en cuanto se resuelvan esos problemas.*  
  


