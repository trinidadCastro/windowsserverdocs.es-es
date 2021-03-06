---
title: Se recomienda la compresión para el tráfico de replicación
description: Obtenga información sobre qué hacer cuando se descomprime el tráfico de replicación enviado a través de la red desde el servidor principal al servidor réplica.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
ms.date: 8/16/2016
ms.openlocfilehash: 41825e6484c7cef85b11795a68df501b13c4881e
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833650"
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

## <a name="issue"></a>Problema
*Se descomprimirá el tráfico de replicación enviado a través de la red desde el servidor principal al servidor réplica.*

## <a name="impact"></a>Impacto
*El tráfico de replicación usará más ancho de banda de lo necesario. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Configure la réplica de Hyper-V para comprimir los datos transmitidos a través de la red en la configuración de la máquina virtual en el administrador de Hyper-V. También puede usar herramientas fuera de Hyper-V para realizar la compresión.*



