---
title: La replicación se pausó para una o varias máquinas virtuales en este servidor
description: Obtenga información sobre qué hacer cuando se pausa la replicación de una o varias máquinas virtuales. Mientras la máquina virtual principal esté en pausa, los cambios que se produzcan se acumularán y se enviarán a la máquina virtual de réplica una vez que se reanude la replicación.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
ms.date: 8/16/2016
ms.openlocfilehash: bc8a173773217ea00c47080ded74901f70384b2a
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846121"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La replicación se pausó para una o varias máquinas virtuales en este servidor

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operations|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*La replicación se pausa para una o varias de las máquinas virtuales. Mientras la máquina virtual principal esté en pausa, los cambios que se produzcan se acumularán y se enviarán a la máquina virtual de réplica una vez que se reanude la replicación.*

## <a name="impact"></a>Impacto
*Siempre que se pausa la replicación, los cambios acumulados que se produzcan en la máquina virtual principal consumirán espacio en disco disponible en el servidor principal. Después de reanudar la replicación, puede haber una gran ráfaga del tráfico de red al servidor de réplicas. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Confirme que se ha diseñado la pausa de la replicación. Si la replicación se pausó para solucionar poco espacio en disco o la conectividad de red, reanude la replicación en cuanto se resuelvan esos problemas.*



