---
title: Afinidad de clúster
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 03/07/2019
description: En este artículo se describe los niveles de afinidad y antiAffinity del clúster de conmutación por error
ms.openlocfilehash: a38d53f6aed1ca634d41822f4486779f6d279ec0
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476048"
---
# <a name="cluster-affinity"></a>Afinidad de clúster

> Se aplica a: Windows Server 2019, Windows Server 2016

Un clúster de conmutación por error puede contener numerosas funciones que pueden transferirse entre los nodos y ejecutar.  Hay veces cuando no deben ejecutar determinados roles (es decir, las máquinas virtuales, grupos de recursos, etcetera) en el mismo nodo.  Esto podría ser debido a consumo de recursos, el uso de memoria, etcetera.  Por ejemplo, hay dos máquinas virtuales que están en memoria y CPU considerable y si las dos máquinas virtuales se ejecutan en el mismo nodo, una o ambas de las máquinas virtuales podrían tener problemas de rendimiento de impacto.  Este artículo explicará clúster antiaffinity niveles y cómo usarlos.

## <a name="what-is-affinity-and-antiaffinity"></a>¿Qué es la afinidad y AntiAffinity?

La afinidad es para configurar una regla que establece una relación entre dos o más roles (i, e, las máquinas virtuales, grupos de recursos, etcetera) para mantenerlos juntos.  AntiAffinity es el mismo, pero se usa para probar y mantener los roles especificados además entre sí.  Clústeres de conmutación por error use AntiAffinity para sus roles.  Más concretamente, el [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames) parámetro definido en los roles, por lo que no se ejecutan en el mismo nodo.  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

Al examinar las propiedades de un grupo, hay el parámetro AntiAffinityClassNames y está en blanco de forma predeterminada.  En los ejemplos siguientes, deben estar separados Group1 y Group2 se ejecuten en el mismo nodo.  Para ver la propiedad, sería el comando de PowerShell y el resultado:

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

Puesto que AntiAffinityClassNames no se definen de forma predeterminada, estos pueden roles separados o ejecución juntos.  El objetivo es mantener puedan separarse.  El valor de AntiAffinityClassNames puede ser lo que quiera que sean, solo tienen que ser el mismo.  Supongamos que Group1 y Group2 son controladores de dominio que se ejecutan en máquinas virtuales y podrían mejorarse que se ejecutan en distintos nodos.  Dado que son controladores de dominio, usaré el controlador de dominio para el nombre de clase.  Para establecer el valor, el comando de PowerShell y los resultados serían:

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

Ahora que están establecidos, agrupación en clústeres de conmutación por error intentará mantenerlas separadas.  

El parámetro AntiAffinityClassName es un bloque "soft".  Es decir, intentará mantenerlas separadas, pero si no es posible, permitirá aún que se ejecuten en el mismo nodo.  Por ejemplo, los grupos se ejecutan en un clúster de conmutación por error de dos nodos.  Si necesita un nodo deje de funcionar por mantenimiento, significaría que ambos grupos sería en marcha en el mismo nodo.  En este caso, sería correcto tener esto.  No puede ser más adecuado, pero ambas máquinas virtial le sigue ejecutando dentro de los intervalos de un rendimiento aceptable.

## <a name="i-need-more"></a>Necesito más

Como se mencionó, AntiAffinityClassNames es un bloqueo suave.  Pero ¿qué ocurre si se necesita un bloque de disco duro?  Las máquinas virtuales no se puede ejecutar en él mismo nodo; en caso contrario, el impacto de rendimiento se producirse y hacer que algunos servicios posiblemente deje de funcionar.

Para esos casos, hay una propiedad de clúster adicional de ClusterEnforcedAntiAffinity.  Este nivel antiaffinity impedirá a toda costa cualquiera de los mismos valores AntiAffinityClassNames que se ejecutan en el mismo nodo.

Para ver la propiedad y valor, sería el comando de PowerShell (y el resultado):

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

El valor de "0" significa que está deshabilitado y no para su cumplimiento.  El valor de "1" le permite y es el bloque de disco duro.  Para habilitar este bloque de disco duro, el comando (y el resultado) son:

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

Cuando se establecen ambos, el grupo se impedirá juntos entran en línea.  Si están en el mismo nodo, esto es lo que vería en el Administrador de clústeres de conmutación por error.

![Afinidad de clúster](media\Cluster-Affinity\Cluster-Affinity-1.png)

En una lista de PowerShell de los grupos, vería esto:

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>Comentarios adicionales

- Asegúrese de que usa el valor AntiAffinity adecuado según las necesidades.
- Tenga en cuenta que en un escenario de dos nodos y ClusterEnforcedAntiAffinity, si un nodo está inactivo, ambos grupos no se ejecutará.  

- El uso de propietarios preferidos en grupos se puede combinar con AntiAffinity en un clúster de tres o más nodos.
- La configuración de AntiAffinityClassNames y ClusterEnforcedAntiAffinity sólo llevará a cabo después de un reciclaje de los recursos. I.E. puede establecer, pero si ambos grupos están en línea en el mismo nodo cuando se establece, ambos seguirán permanezcan en línea.



