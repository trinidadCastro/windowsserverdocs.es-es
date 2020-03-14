---
title: Quitar servidores en Espacios de almacenamiento directo
ms.assetid: 9d8499a7-1307-473d-9f00-8a051164fad2
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
description: Cómo quitar servidores de un clúster de Espacios de almacenamiento directo en Windows Server.
ms.date: 2/5/2017
ms.localizationpriority: medium
ms.openlocfilehash: ce8caef2b51279c97cc012045750b7a73d97a4ba
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322317"
---
# <a name="removing-servers-in-storage-spaces-direct"></a>Quitar servidores en Espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se describe cómo quitar servidores en [Espacios de almacenamiento directo](storage-spaces-direct-overview.md) mediante PowerShell.

## <a name="remove-a-server-but-leave-its-drives"></a>Quitar un servidor, pero dejar las unidades

Si tienes pensado volver a agregar el servidor al clúster pronto, o si quieres mover las unidades a otro servidor para mantenerlas, puedes quitar el servidor del clúster *sin* quitar sus unidades del grupo de almacenamiento. Este es el comportamiento predeterminado, si usas el Administrador de clústeres de conmutación por error para quitar el servidor.

Usa el cmdlet [Remove-ClusterNode](https://technet.microsoft.com/library/hh847251.aspx) de PowerShell:

```PowerShell
Remove-ClusterNode <Name>
```

Este cmdlet lo logra con rapidez, independientemente de las consideraciones de capacidad, ya que el grupo de almacenamiento "recuerda" las unidades que faltan y espera que vuelvan. No hay ningún movimiento de datos de extracción de las unidades que faltan. Si bien seguirán faltando, su **OperationalStatus** aparecerá como "Comunicación perdida", y los volúmenes mostrarán "Incompleto".

Cuando vuelvan las unidades, se detectarán y volverán a asociarse con el grupo automáticamente, incluso si ahora están en un nuevo servidor.

   >[!WARNING]
   > No distribuyas las unidades con datos agrupados de un servidor a otros varios servidores. Por ejemplo, si se produce un error en un servidor con diez unidades (por ejemplo, por un problema en la placa base o en la unidad de arranque), **puedes** mover las diez unidades a un nuevo servidor, pero **no** puedes mover cada una por separado a varios servidores distintos.

## <a name="remove-a-server-and-its-drives"></a>Quitar un servidor y las unidades

Si quieres quitar un servidor del clúster de forma permanente (a veces denominado "escalado"), puedes quitar el servidor del clúster *y* quitar las unidades del grupo de almacenamiento.

Usa el cmdlet **Remove-ClusterNode** con la marca **- CleanUpDisks** opcional:

```PowerShell
Remove-ClusterNode <Name> -CleanUpDisks
```

Este cmdlet puede tardar mucho tiempo (en ocasiones, muchas horas) en ejecutarse porque Windows debe mover todos los datos almacenados en ese servidor a otros servidores del clúster. Una vez que se completa esto, las unidades se quitan de manera permanente del grupo de almacenamiento, lo que devuelve los volúmenes afectados a un estado correcto.

### <a name="requirements"></a>Requisitos

Para escalar (quitar un servidor *y* sus unidades) de manera permanente, el clúster debe cumplir los siguientes dos requisitos. Si no es así, el cmdlet **Remove-ClusterNode -CleanUpDisks** devolverá un error de inmediato, antes de que comience cualquier movimiento de datos, para minimizar las interrupciones.

#### <a name="enough-capacity"></a>Capacidad suficiente

En primer lugar, debe tener suficiente capacidad de almacenamiento en los servidores restantes para dar cabida a todos los volúmenes.

Por ejemplo, si tienes cuatro servidores, cada uno con 10 unidades de 1 TB, tienes 40 TB de capacidad de almacenamiento físico total. Después de quitar un servidor y todas sus unidades, quedarás con 30 TB de capacidad. Si las superficies de los volúmenes son de más de 30 TB en conjunto, no cabrán en los servidores restantes, por lo que el cmdlet devolverá un error y no moverá ningún dato.

#### <a name="enough-fault-domains"></a>Dominios de error suficientes

En segundo lugar, debes tener suficientes dominios de error (por lo general, servidores) para proporcionar la resistencia de los volúmenes.

Por ejemplo, si los volúmenes usan la creación de reflejos triple en el nivel de servidor para resistencia, no pueden tener un estado completamente correcto, a menos que tengas al menos tres servidores. Si tienes exactamente tres servidores y luego intentas quitar uno y todas sus unidades, el cmdlet devolverá un error y no moverá ningún dato.

En esta tabla se muestra el número mínimo de dominios de error necesario para cada tipo de resistencia.

|    Resistencia          |    Mínimo obligatorio de dominios de error   |
|------------------------|-------------------------------------|
|    Reflejo doble      |    2                                |
|    Reflejo triple    |    3                                |
|    Paridad doble         |    4                                |

   >[!NOTE]
   > No hay problemas si tienes menos servidores por un momento, como durante errores o mantenimiento. Sin embargo, para que los volúmenes vuelvan a un buen estado por completo, debes tener el número mínimo de servidores que se indica anteriormente.

## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
