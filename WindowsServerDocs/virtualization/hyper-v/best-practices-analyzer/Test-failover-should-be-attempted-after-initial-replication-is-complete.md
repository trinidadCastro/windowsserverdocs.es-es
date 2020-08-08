---
title: La conmutación por error de prueba debe intentarse una vez completada la replicación inicial
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 52b69c2e483e8f3b07d64a1a4ccf579018e1a3db
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960548"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>La conmutación por error de prueba debe intentarse una vez completada la replicación inicial

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operaciones|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="problem"></a>Problema
*No se ha realizado ninguna conmutación por error de prueba en al menos un mes.*

## <a name="impact"></a>Impacto
*No se confirma que una conmutación por error planeada o no planeada se realizará correctamente o las operaciones de carga de trabajo continuarán correctamente después de una conmutación por error. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Use el administrador de Hyper-V para realizar una conmutación por error de prueba.*



