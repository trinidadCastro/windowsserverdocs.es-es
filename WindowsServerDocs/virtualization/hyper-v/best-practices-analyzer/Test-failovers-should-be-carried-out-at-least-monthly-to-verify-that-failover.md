---
title: Las conmutaciones por error de prueba deben realizarse al menos mensualmente para comprobar que la conmutación por error se realizará correctamente y que las cargas de trabajo de máquina virtual funcionarán según lo previsto tras la conmutación por error
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 57a8aa50-e59e-4a4b-8571-1099d5a8eee4
ms.date: 8/16/2016
ms.openlocfilehash: 4ec97d55f1a6caf33b1b46d0a6bdb4e99005c971
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90744098"
---
# <a name="test-failovers-should-be-carried-out-at-least-monthly-to-verify-that-failover-will-succeed-and-that-virtual-machine-workloads-will-operate-as-expected-after-failover"></a>Las conmutaciones por error de prueba deben realizarse al menos mensualmente para comprobar que la conmutación por error se realizará correctamente y que las cargas de trabajo de máquina virtual funcionarán según lo previsto tras la conmutación por error

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
*No se ha realizado ninguna conmutación por error de prueba en al menos un mes.*

## <a name="impact"></a>Impacto
*No se confirma que una conmutación por error planeada o no planeada se realizará correctamente o las operaciones de carga de trabajo continuarán correctamente después de una conmutación por error. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Use el administrador de Hyper-V para realizar una conmutación por error de prueba.*



