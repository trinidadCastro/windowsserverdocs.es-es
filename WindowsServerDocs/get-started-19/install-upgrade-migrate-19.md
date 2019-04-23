---
title: Instalar | Actualizar | Migrar a Windows Server de 2019
description: Cómo una instalación limpia, actualización local o migrar a Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 58c363fc0a1e336519bc6ec4276651345cc2b5eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868716"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>Instalar | Actualizar | Migrar a Windows Server de 2019

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> El soporte técnico ampliado para Windows Server 2008 R2 y Windows Server 2008 termina en enero de 2020. [Obtenga información sobre las opciones de actualización](http://aka.ms/upgradecenter).

¿Ha llegado la hora de pasar a una versión más reciente de Windows Server? En función de la versión que estés utilizando en este momento, tienes muchas opciones para hacerlo.

## <a name="clean-install"></a>Instalación limpia
Si desea mover de una versión anterior de Windows Server a Windows Server 2019 en el mismo hardware, debe hacer una **instalación limpia**, donde sólo tiene que instalar el sistema operativo más reciente directamente sobre la anterior en el mismo hardware, Eliminando, por tanto, el sistema operativo anterior. Esta es la forma más sencilla, pero antes tendrás que hacer una copia de seguridad de los datos y planear la reinstalación de las aplicaciones. Hay algunos aspectos que debe tener en cuenta, como los requisitos del sistema, así que asegúrese de comprobar los detalles de [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) , y [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Actualización en contexto
Si desea mantener el mismo hardware y todos los roles de servidor ha configurado sin acoplar el servidor, conviene hacer un **actualización In situ**, por el que se pasa de un antiguo sistema a una versión más reciente operativo, mantener la configuración de servidor las funciones y datos intactos. Por ejemplo, si el servidor se está ejecutando Windows Server 2012 R2, puede actualizar a Windows Server 2016 o Windows Server 2019. Sin embargo, no todos los sistemas operativos antiguos tiene una ruta de actualización a todas las versiones más recientes. Consulte el diagrama siguiente para las rutas de actualización disponibles:

![Diagrama de las rutas de actualización en contexto de Windows Server](media/upgrade-paths.png)

Para obtener instrucciones paso a paso acerca de la actualización, visite la [centro de actualización de Windows Server](http://aka.ms/upgradecenter):

<a href="http://aka.ms/upgradecenter"><img src="media/upgrade-center.png" alt="Screenshot of Windows Upgrade Center" title="Centro de actualización de Windows Server"></a>

## <a name="cluster-os-rolling-upgrade"></a>Actualización del sistema operativo sucesiva de clúster
Actualización gradual de clúster del sistema operativo permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 y Windows Server 2016 sin detener la función Hyper-V o las cargas de trabajo de servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Esta nueva característica se describe con más detalle en [Actualización gradual del sistema operativo del clúster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migración

Migración de Windows Server es cuando se mueve un rol o característica a la vez desde un equipo de origen que ejecuta Windows Server a otro equipo de destino que ejecuta Windows Server, la misma versión o una versión más reciente. Para estos fines, la migración se define como mover un rol o una característica y sus datos a un equipo diferente, no a actualizar la característica en el mismo equipo. 

## <a name="license-conversion"></a>Conversión de licencia
En algunas versiones de los sistemas operativos, es posible convertir una edición concreta de la versión a otra edición de la misma versión en un solo paso, con un sencillo comando y la clave de licencia correspondiente. Esto se denomina **conversión de licencia**. Por ejemplo, si el servidor ejecuta Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter. En algunas versiones de Windows Server, es posible convertir también libremente entre las versiones de OEM, de licencia por volumen y comerciales con el mismo comando y la clave apropiada.


 
 
