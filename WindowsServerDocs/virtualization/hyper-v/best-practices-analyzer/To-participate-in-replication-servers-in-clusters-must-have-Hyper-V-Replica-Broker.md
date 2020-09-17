---
title: Para participar en la replicación, los servidores de los clústeres de conmutación por error deben tener configurado un agente de réplicas de Hyper-V
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
ms.date: 8/16/2016
ms.openlocfilehash: 7fdd6167561be4922540a0def1f91f70b519aa0f
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746130"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Para participar en la replicación, los servidores de los clústeres de conmutación por error deben tener configurado un agente de réplicas de Hyper-V

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*En el caso de los clústeres de conmutación por error, réplica de Hyper-V requiere el uso de un nombre de agente de réplicas de Hyper-V en lugar de un nombre de servidor individual.*

## <a name="impact"></a>Impacto
*Si la máquina virtual se mueve a otro nodo de clúster de conmutación por error, la replicación no puede continuar.*

## <a name="resolution"></a>Solución
*Use Administrador de clústeres de conmutación por error para configurar el agente de réplicas de Hyper-V. En el administrador de Hyper-V, asegúrese de que la configuración de replicación usa el nombre del agente de réplicas de Hyper-V como nombre del servidor.*



