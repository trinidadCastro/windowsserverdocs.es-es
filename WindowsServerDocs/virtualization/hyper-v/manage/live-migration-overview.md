---
title: Información general sobre la migración en vivo
description: Proporciona información general sobre la funcionalidad de migración en vivo en Windows Server 2016.
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
ms.author: benarm
author: BenjaminArmstrong
ms.date: 06/27/2017
ms.openlocfilehash: 46768ced2b493ff68df945739fb0b3f920846c7d
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746620"
---
# <a name="live-migration-overview"></a>Información general sobre la migración en vivo

La migración en vivo es una característica de Hyper-V en Windows Server.  Permite trasladar de forma transparente Virtual Machines de ejecución de un host de Hyper-V a otro sin que se perciba tiempo de inactividad.  La principal ventaja de la migración en vivo es la flexibilidad; la ejecución de Virtual Machines no está asociada a un único equipo host.  Esto permite acciones como la purga de un host específico de Virtual Machines antes de la retirada o la actualización.  Cuando se empareja con el clúster de conmutación por error de Windows, la migración en vivo permite la creación de sistemas de alta disponibilidad y tolerancia a errores.

## <a name="related-technologies-and-documentation"></a>Documentación y tecnologías relacionadas

La migración en vivo se usa a menudo junto con algunas tecnologías relacionadas, como clústeres de conmutación por error y System Center Virtual Machine Manager.  Si usa Migración en vivo a través de estas tecnologías, aquí encontrará punteros a la documentación más reciente:
* [Clústeres de conmutación por error](../../../failover-clustering/failover-clustering-overview.md) (Windows Server 2016)
* [System Center Virtual Machine Manager](/system-center/vmm/) (System Center 2016)

Si usa versiones anteriores de Windows Server, o necesita detalles sobre las características introducidas en versiones anteriores de Windows Server, estos son los punteros a la documentación histórica:
* [Migración en vivo](/previous-versions/windows/it-pro/microsoft-hyper-v-server-2008-R2/ee815293(v=ws.10)) (Windows Server 2008 R2)
* [Migración en vivo](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831435(v=ws.11)) (Windows Server 2012 R2)
* [Clústeres de conmutación por error](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831579(v=ws.11)) (Windows Server 2012 R2)
* [Clústeres de conmutación por error](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff182338(v=ws.10)) (Windows Server 2008 R2)
* [System Center Virtual Machine Manager](/previous-versions/system-center/system-center-2012-R2/gg610610(v=sc.12)) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Migración en vivo en Windows Server 2016

En Windows Server 2016, hay menos restricciones en la implementación de la migración en vivo.  Ahora funciona sin clústeres de conmutación por error.  Otras funciones permanecen sin cambios respecto a las versiones anteriores de Migración en vivo.  Para obtener más información sobre la configuración y el uso de la migración en vivo sin clústeres de conmutación por error:
* [Configurar hosts para la migración en vivo sin clústeres de conmutación por error](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [Usar la migración en vivo sin clústeres de conmutación por error para migrar una máquina virtual](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)