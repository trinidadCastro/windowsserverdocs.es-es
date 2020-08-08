---
title: Se recomienda la compresión para el tráfico de replicación
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2184a2ec0441af7fdbfc7566d783bee82f6bbbb9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954532"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>Se recomienda la compresión para el tráfico de replicación

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
*Se descomprimirá el tráfico de replicación enviado a través de la red desde el servidor principal al servidor réplica.*

## <a name="impact"></a>Impacto
*El tráfico de replicación usará más ancho de banda de lo necesario. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Configure la réplica de Hyper-V para comprimir los datos transmitidos a través de la red en la configuración de la máquina virtual en el administrador de Hyper-V. También puede usar herramientas fuera de Hyper-V para realizar la compresión.*



