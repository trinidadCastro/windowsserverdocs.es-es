---
title: Configurar una directiva para limitar el tráfico de replicación en la red
description: Obtenga información acerca de qué hacer cuando es posible que no haya un límite en la cantidad de ancho de banda de red que puede consumir la replicación.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
ms.date: 8/16/2016
ms.openlocfilehash: 69a03e85dd410f1f3b59daaf97ff97c2db5c7d48
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834850"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurar una directiva para limitar el tráfico de replicación en la red

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Es posible que no haya un límite en la cantidad de ancho de banda de red que puede consumir la replicación.*

## <a name="impact"></a>Impacto
*El ancho de banda de red podría estar completamente dominado por el tráfico de replicación, lo que afecta a otras actividades de red críticas. Esto afecta a los siguientes puertos:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Si usa otro método para limitar el tráfico de red, puede pasarlo por alto. De lo contrario, use el editor de directiva de grupo para configurar una directiva que limitará el tráfico de red al puerto pertinente del servidor réplica.*




