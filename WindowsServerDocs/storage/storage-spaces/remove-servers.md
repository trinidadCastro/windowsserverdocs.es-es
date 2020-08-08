---
title: Quitar servidores en Espacios de almacenamiento directo
ms.assetid: 9d8499a7-1307-473d-9f00-8a051164fad2
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
description: Cómo quitar servidores de un clúster de Espacios de almacenamiento directo en Windows Server.
ms.date: 2/5/2017
ms.localizationpriority: medium
ms.openlocfilehash: cc6ca368be71b24b8a552051c6edc2b7220dd57b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971072"
---
# <a name="removing-servers-in-storage-spaces-direct"></a>Quitar servidores en Espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se describe cómo quitar servidores de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) mediante PowerShell.

## <a name="remove-a-server-but-leave-its-drives"></a>Quitar un servidor y dejar sus unidades

Si piensa volver a agregar el servidor al clúster en breve, o si tiene previsto mantener sus unidades moviéndolos a otro servidor, puede quitar el servidor del clúster *sin* quitar sus unidades del bloque de almacenamiento. Este es el comportamiento predeterminado si usa Administrador de clústeres de conmutación por error para quitar el servidor.

Use el cmdlet [Remove-ClusterNode](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831694(v=ws.11)) en PowerShell:

```PowerShell
Remove-ClusterNode <Name>
```

Este cmdlet se ejecuta rápidamente, independientemente de las consideraciones de capacidad, ya que el grupo de almacenamiento "recuerda" las unidades que faltan y espera que vuelvan. No se produce el traslado de los datos de las unidades que faltan. Aunque permanecen ausentes, su **OperationalStatus** se mostrará como "pérdida de la comunicación" y los volúmenes mostrarán "incompletos".

Cuando las unidades vuelven, se detectan automáticamente y se vuelven a asociar con el grupo, incluso si ahora están en un nuevo servidor.

   >[!WARNING]
   > No distribuya unidades con datos de grupo de un servidor en varios servidores. Por ejemplo, si se produce un error en un servidor con diez unidades de disco (por ejemplo, porque la placa base o la unidad de arranque no se pudo completar), puede mover las diez unidades a un servidor nuevo, pero **no** **puede mover cada** una de ellas por separado en otros servidores diferentes.

## <a name="remove-a-server-and-its-drives"></a>Quitar un servidor y sus unidades

Si desea quitar permanentemente un servidor del clúster (a veces conocido como escalado horizontal), puede quitar el servidor del clúster *y* quitar sus unidades del bloque de almacenamiento.

Use el cmdlet **Remove-ClusterNode** con la marca opcional **-CleanUpDisks** :

```PowerShell
Remove-ClusterNode <Name> -CleanUpDisks
```

Este cmdlet puede tardar mucho tiempo (en ocasiones, muchas horas) en ejecutarse porque Windows debe trasladar todos los datos almacenados en ese servidor a otros servidores del clúster. Una vez que se completa, las unidades se quitan permanentemente del grupo de almacenamiento y devuelven los volúmenes afectados a un estado correcto.

### <a name="requirements"></a>Requisitos

Para reducir de forma permanente (quitar un servidor *y* sus unidades), el clúster debe cumplir los dos requisitos siguientes. Si no es así, el cmdlet **Remove-ClusterNode-CleanUpDisks** devolverá un error inmediatamente, antes de que se inicie cualquier movimiento de datos, para minimizar la interrupción.

#### <a name="enough-capacity"></a>Capacidad suficiente

En primer lugar, debe tener suficiente capacidad de almacenamiento en los servidores restantes para dar cabida a todos los volúmenes.

Por ejemplo, si tiene cuatro servidores, cada uno con 10 unidades de 1 TB, tiene 40 TB de capacidad total de almacenamiento físico. Después de quitar un servidor y todas sus unidades, tendrá 30 TB de capacidad restante. Si el tamaño de los volúmenes es superior a 30 TB, no caben en el resto de servidores, por lo que el cmdlet devolverá un error y no moverá ningún dato.

#### <a name="enough-fault-domains"></a>Dominios de error suficientes

En segundo lugar, debe tener suficientes dominios de error (normalmente servidores) para proporcionar la resistencia de los volúmenes.

Por ejemplo, si los volúmenes utilizan la creación de reflejo triple en el nivel de servidor para lograr resistencia, no pueden ser totalmente correctos a menos que tenga al menos tres servidores. Si tiene exactamente tres servidores y, después, intenta quitar una y todas sus unidades, el cmdlet devolverá un error y no moverá ningún dato.

En esta tabla se muestra el número mínimo de dominios de error necesarios para cada tipo de resistencia.

|    Resistencia          |    Dominios de error mínimos requeridos   |
|------------------------|-------------------------------------|
|    Reflejo bidireccional      |    2                                |
|    Reflejo triple    |    3                                |
|    Paridad dual         |    4                                |

   >[!NOTE]
   > Es correcto tener menos servidores, como por ejemplo, durante errores o mantenimiento. Sin embargo, para que los volúmenes vuelvan a un estado totalmente correcto, debe tener el número mínimo de servidores enumerados anteriormente.

## <a name="additional-references"></a>Referencias adicionales

- [Introducción a Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
