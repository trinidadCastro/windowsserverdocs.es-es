---
title: Afinidad de clústeres
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/07/2019
description: En este artículo se describen los niveles de afinidad y antiafinidad de clústeres de conmutación por error
ms.openlocfilehash: 9e0c16b376201567552ef959e045027527c7c1d1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965562"
---
# <a name="cluster-affinity"></a>Afinidad de clústeres

> Se aplica a: Windows Server 2019, Windows Server 2016

Un clúster de conmutación por error puede contener numerosos roles que pueden moverse entre nodos y ejecutarse. Hay ocasiones en que determinados roles (es decir, máquinas virtuales, grupos de recursos, etc.) no deben ejecutarse en el mismo nodo.  Esto puede deberse al consumo de recursos, al uso de memoria, etc.  Por ejemplo, hay dos máquinas virtuales con un uso intensivo de memoria y CPU, y si las dos máquinas virtuales se ejecutan en el mismo nodo, una de las máquinas virtuales, o ambas, podrían tener problemas de impacto en el rendimiento.  En este artículo se explican los niveles de antiafinidad de clústeres y cómo puede usarlos.

## <a name="what-is-affinity-and-antiaffinity"></a>¿Qué es la afinidad y la antiafinidad?

La afinidad es una regla que se debe configurar y que establece una relación entre dos o más roles (i, e, máquinas virtuales, grupos de recursos, etc.) para mantenerlos juntos.  La antiafinidad es la misma, pero se utiliza para intentar mantener los roles especificados entre sí. Los clústeres de conmutación por error usan la antiafinidad para sus roles.  Más concretamente, el parámetro [AntiAffinityClassNames](/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames) definido en los roles para que no se ejecuten en el mismo nodo.

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

Al examinar las propiedades de un grupo, existe el parámetro AntiAffinityClassNames y está en blanco como valor predeterminado.  En los ejemplos siguientes, Grupo1 y grupo2 deben estar separados de ejecutarse en el mismo nodo.  Para ver la propiedad, el comando de PowerShell y el resultado serían:

```powershell
Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}
```

Dado que AntiAffinityClassNames no se define como valor predeterminado, estos roles se pueden ejecutar juntos o separar.  El objetivo es mantenerlos separados.  El valor de AntiAffinityClassNames puede ser el que desee que sea, solo tiene que ser el mismo.  Suponga que Grupo1 y grupo2 son controladores de dominio que se ejecutan en máquinas virtuales y que se ejecutarán mejor en nodos diferentes.  Puesto que son controladores de dominio, usaremos DC como nombre de clase.  Para establecer el valor, el comando de PowerShell y los resultados serían:

```powershell
$AntiAffinity = New-Object System.Collections.Specialized.StringCollection
$AntiAffinity.Add("DC")
(Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
(Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

$AntiAffinity = New-Object System.Collections.Specialized.StringCollection
$AntiAffinity.Add("DC")
(Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
(Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}
```

Ahora que están configurados, los clústeres de conmutación por error intentarán mantenerlos separados.

El parámetro establecer antiaffinityclassname es un bloque "soft".  Es decir, tratará de mantenerlos separados, pero si no es posible, seguirá pudiendo ejecutarse en el mismo nodo.  Por ejemplo, los grupos se ejecutan en un clúster de conmutación por error de dos nodos.  Si un nodo necesita dejar de funcionar para el mantenimiento, significaría que ambos grupos estarán funcionando en el mismo nodo.  En este caso, sería correcto tener esto.  Puede que no sea la más idónea, pero ambas máquinas virtuales seguirán ejecutándose dentro de intervalos de rendimiento aceptables.

## <a name="i-need-more"></a>Necesito más

Como se mencionó, AntiAffinityClassNames es un bloque blando.  Pero, ¿qué ocurre si se necesita un bloque duro?  Las máquinas virtuales no se pueden ejecutar en el mismo nodo; de lo contrario, se producirá un impacto en el rendimiento y algunos servicios probablemente dejarán de funcionar.

En esos casos, hay una propiedad de clúster adicional de ClusterEnforcedAntiAffinity.  Este nivel de antiafinidad impedirá que se ejecuten cualquiera de los mismos valores de AntiAffinityClassNames en el mismo nodo.

Para ver la propiedad y el valor, el comando de PowerShell (y el resultado) sería:

```powershell
Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0
```

El valor de "0" significa que está deshabilitado y no se aplicará.  El valor de "1" lo habilita y es el bloque duro.  Para habilitar este bloque duro, el comando (y el resultado) es:

```powershell
(Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1
```

Cuando se establezcan ambos, se impedirá que el grupo se ponga en línea juntos.  Si están en el mismo nodo, esto es lo que se verá en Administrador de clústeres de conmutación por error.

![Afinidad de clústeres](media/Cluster-Affinity/Cluster-Affinity-1.png)

En una lista de PowerShell de los grupos, verá lo siguiente:

```powershell
Get-ClusterGroup

Name       State
----       -----
Group1     Offline(Anti-Affinity Conflict)
Group2     Online
```

## <a name="additional-comments"></a>Comentarios adicionales

- Asegúrese de que está usando la configuración de antiafinidad adecuada en función de las necesidades.
- Tenga en cuenta que en un escenario de dos nodos y ClusterEnforcedAntiAffinity, si un nodo está inactivo, no se ejecutarán ambos grupos.

- El uso de los propietarios preferidos en los grupos se puede combinar con la antiafinidad en un clúster de tres o más nodos.
- La configuración de AntiAffinityClassNames y ClusterEnforcedAntiAffinity solo tendrá lugar después de un reciclaje de los recursos. es decir,. puede establecerlos, pero si ambos grupos están en línea en el mismo nodo cuando se establecen, seguirán estando en línea.
