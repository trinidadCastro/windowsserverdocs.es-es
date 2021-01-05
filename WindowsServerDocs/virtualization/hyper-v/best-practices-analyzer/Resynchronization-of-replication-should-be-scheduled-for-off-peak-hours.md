---
title: La resincronización de la replicación debe programarse para horas de poca actividad
description: Obtenga información acerca de qué hacer cuando la resincronización de la replicación de las máquinas virtuales principales no está programada para horas de poca actividad.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
ms.date: 8/16/2016
ms.openlocfilehash: 3669abf4da79db82d0ba7ca985b886a02a180ea9
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845813"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>La resincronización de la replicación debe programarse para horas de poca actividad

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
*La resincronización de la replicación de las máquinas virtuales principales no está programada para horas de poca actividad.*

## <a name="impact"></a>Impacto
*Cuanto más tiempo una máquina virtual se encuentra en un estado que requiere resincronización, más tiempo aumentan los archivos de registro de replicación y se producen cambios más sin replicar en las máquinas virtuales principales. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Use el administrador de Hyper-V para modificar la configuración de replicación de la máquina virtual para realizar la resincronización automáticamente durante las horas de menor actividad.*



