---
title: La resincronización de la replicación debe programarse para horas de poca actividad
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: dad983e19df328946f3bd7ec59f68ee3e9f8bfcc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939011"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>La resincronización de la replicación debe programarse para horas de poca actividad

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
*La resincronización de la replicación de las máquinas virtuales principales no está programada para horas de poca actividad.*

## <a name="impact"></a>Impacto
*Cuanto más tiempo una máquina virtual se encuentra en un estado que requiere resincronización, más tiempo aumentan los archivos de registro de replicación y se producen cambios más sin replicar en las máquinas virtuales principales. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Use el administrador de Hyper-V para modificar la configuración de replicación de la máquina virtual para realizar la resincronización automáticamente durante las horas de menor actividad.*



