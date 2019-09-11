---
title: Información general de la migración en vivo
description: Proporciona información general sobre la funcionalidad de migración en vivo en Windows Server 2016.
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 2842435f1bdb0aeb82bbcf1ae02be66e242dc9eb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872053"
---
# <a name="live-migration-overview"></a>Información general de la migración en vivo

La migración en vivo es una característica de Hyper-V en Windows Server.  Permite trasladar de forma transparente Virtual Machines de ejecución de un host de Hyper-V a otro sin que se perciba tiempo de inactividad.  La principal ventaja de la migración en vivo es la flexibilidad; la ejecución de Virtual Machines no está asociada a un único equipo host.  Esto permite acciones como la purga de un host específico de Virtual Machines antes de la retirada o la actualización.  Cuando se empareja con el clúster de conmutación por error de Windows, la migración en vivo permite la creación de sistemas de alta disponibilidad y tolerancia a errores. 

## <a name="related-technologies-and-documentation"></a>Documentación y tecnologías relacionadas

La migración en vivo se usa a menudo junto con algunas tecnologías relacionadas, como clústeres de conmutación por error y System Center Virtual Machine Manager.  Si usa Migración en vivo a través de estas tecnologías, aquí encontrará punteros a la documentación más reciente:
* [Clústeres de conmutación por error](../../../failover-clustering/failover-clustering-overview.md) (Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

Si usa versiones anteriores de Windows Server, o necesita detalles sobre las características introducidas en versiones anteriores de Windows Server, estos son los punteros a la documentación histórica: 
* [Migración en vivo](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx) (Windows Server 2008 R2)  
* [Migración en vivo](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx) (Windows Server 2012 R2) 
* [Clústeres de conmutación por error](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx) (Windows Server 2012 R2)
* [Clústeres de conmutación por error](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx) (Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Migración en vivo en Windows Server 2016

En Windows Server 2016, hay menos restricciones en la implementación de la migración en vivo.  Ahora funciona sin clústeres de conmutación por error.  Otras funciones permanecen sin cambios respecto a las versiones anteriores de Migración en vivo.  Para obtener más información sobre la configuración y el uso de la migración en vivo sin clústeres de conmutación por error: 
* [Configurar hosts para la migración en vivo sin clústeres de conmutación por error](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [Usar la migración en vivo sin clústeres de conmutación por error para migrar una máquina virtual](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)