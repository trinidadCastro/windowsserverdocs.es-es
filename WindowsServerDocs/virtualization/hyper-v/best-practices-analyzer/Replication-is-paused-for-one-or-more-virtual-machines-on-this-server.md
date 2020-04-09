---
title: La replicación se pausó para una o varias máquinas virtuales en este servidor
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6974016dd564cf19d39a6d83b041020a4e327d2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861818"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La replicación se pausó para una o varias máquinas virtuales en este servidor

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*La replicación se pausa para una o varias de las máquinas virtuales. Mientras la máquina virtual principal esté en pausa, los cambios que se produzcan se acumularán y se enviarán a la máquina virtual de réplica una vez que se reanude la replicación.*  
  
## <a name="impact"></a>Impacto  
*Siempre que se pausa la replicación, los cambios acumulados que se produzcan en la máquina virtual principal consumirán espacio en disco disponible en el servidor principal. Después de reanudar la replicación, puede haber una gran ráfaga del tráfico de red al servidor de réplicas. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Confirme que se ha diseñado la pausa de la replicación. Si la replicación se pausó para solucionar poco espacio en disco o la conectividad de red, reanude la replicación en cuanto se resuelvan esos problemas.*  
  


