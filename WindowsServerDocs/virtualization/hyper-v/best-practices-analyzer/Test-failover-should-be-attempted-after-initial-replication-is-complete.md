---
title: La conmutación por error de prueba debe intentarse una vez completada la replicación inicial
description: Obtenga información sobre qué hacer cuando no haya ninguna conmutación por error de prueba una vez completada la replicación inicial.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
ms.date: 8/16/2016
ms.openlocfilehash: 5f97d6c5d9a44691af22506008dd20cd6f7a45a4
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845847"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>La conmutación por error de prueba debe intentarse una vez completada la replicación inicial

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operations|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="problem"></a>Problema
*No se ha realizado ninguna conmutación por error de prueba en al menos un mes.*

## <a name="impact"></a>Impacto
*No se confirma que una conmutación por error planeada o no planeada se realizará correctamente o las operaciones de carga de trabajo continuarán correctamente después de una conmutación por error. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Use el administrador de Hyper-V para realizar una conmutación por error de prueba.*



