---
title: Información general de la migración en vivo
description: Proporciona información general de la funcionalidad de migración en vivo en Windows Server 2016.
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 2bbe897ffb8b200a72fac5a662e518d4a4be1131
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887856"
---
# <a name="live-migration-overview"></a>Información general de la migración en vivo

Migración en vivo es una característica de Hyper-V en Windows Server.  Permite mover de manera transparente máquinas virtuales en ejecución desde un host de Hyper-V a otro sin percibir tiempo de inactividad.  La principal ventaja de la migración en vivo es la flexibilidad; Máquinas virtuales en ejecución no están vinculados a un único equipo host.  Esto permite acciones como purgar un host específico de máquinas virtuales antes de retirar o actualizarlo.  Cuando se emparejan con los clústeres de conmutación por error de Windows, la migración en vivo permite la creación de alta disponibilidad y el error sistemas con tolerancia a errores. 

## <a name="related-technologies-and-documentation"></a>Documentación y las tecnologías relacionadas

Migración en vivo a menudo se usa junto con unas pocas tecnologías relacionadas como la agrupación en clústeres de conmutación por error y System Center Virtual Machine Manager.  Si usa migración en vivo a través de estas tecnologías, estos son punteros a la documentación más reciente:
* [Agrupación en clústeres de conmutación por error](../../../failover-clustering/failover-clustering-overview.md) (Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

Si usa versiones anteriores de Windows Server, o si necesita obtener más información sobre las características introducidas en versiones anteriores de Windows Server, estos son punteros a la documentación histórico: 
* [Live Migration](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx) (Windows Server 2008 R2)  
* [Migración en vivo](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx) (Windows Server 2012 R2) 
* [Agrupación en clústeres de conmutación por error](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx) (Windows Server 2012 R2)
* [Agrupación en clústeres de conmutación por error](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx) (Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Migración en vivo en Windows Server 2016

En Windows Server 2016, hay menos restricciones en la implementación de la migración en vivo.  Ahora funciona sin agrupación en clústeres de conmutación por error.  Otra funcionalidad permanece sin cambios desde versiones anteriores de la migración en vivo.  Para obtener más información sobre cómo configurar y usar la migración en vivo sin clústeres de conmutación por error: 
* [Configuración de hosts para migración en vivo sin clústeres de conmutación por error](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [Usar la migración en vivo sin clústeres de conmutación por error para mover una máquina virtual](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)