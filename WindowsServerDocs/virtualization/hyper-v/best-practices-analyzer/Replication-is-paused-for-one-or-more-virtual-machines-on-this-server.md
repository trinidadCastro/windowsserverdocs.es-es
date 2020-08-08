---
title: La replicación se pausó para una o varias máquinas virtuales en este servidor
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a40c4d45eea6d0c363cd03d5eef94543ddc1317d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948433"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La replicación se pausó para una o varias máquinas virtuales en este servidor

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operaciones|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia
*La replicación se pausa para una o varias de las máquinas virtuales. Mientras la máquina virtual principal esté en pausa, los cambios que se produzcan se acumularán y se enviarán a la máquina virtual de réplica una vez que se reanude la replicación.*

## <a name="impact"></a>Impacto
*Siempre que se pausa la replicación, los cambios acumulados que se produzcan en la máquina virtual principal consumirán espacio en disco disponible en el servidor principal. Después de reanudar la replicación, puede haber una gran ráfaga del tráfico de red al servidor de réplicas. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Confirme que se ha diseñado la pausa de la replicación. Si la replicación se pausó para solucionar poco espacio en disco o la conectividad de red, reanude la replicación en cuanto se resuelvan esos problemas.*



