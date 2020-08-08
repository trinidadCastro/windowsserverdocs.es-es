---
title: Configure las opciones de TCP/IP de conmutación por error que desea que use la máquina virtual de réplica en caso de una conmutación por error.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 713be5cf428617287e0be0bc65b3e2beb2d11400
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948453"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Configure las opciones de TCP/IP de conmutación por error que desea que use la máquina virtual de réplica en caso de una conmutación por error.

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia
*Las máquinas virtuales de réplica configuradas con una dirección IP estática deben configurarse para usar una dirección IP diferente de su equivalente de máquina virtual principal en caso de conmutación por error.*

## <a name="impact"></a>Impacto
*Es posible que los clientes que usan la carga de trabajo compatible con la máquina virtual principal no puedan conectarse a la máquina virtual de réplica después de una conmutación por error. Además, la dirección IP original de la máquina virtual principal no será válida en la topología de red de la máquina virtual de réplica. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Use el administrador de Hyper-V para configurar la dirección IP que la máquina virtual de réplica debe usar en caso de conmutación por error.*



