---
title: Máquinas virtuales blindadas y tejido protegido
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c06432a039341978956066344710920652187b97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403662"
---
# <a name="guarded-fabric-and-shielded-vms"></a>Máquinas virtuales blindadas y tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Uno de los objetivos más importantes de proporcionar un entorno hospedado es garantizar la seguridad de las máquinas virtuales que se ejecutan en el entorno. Como administrador de empresa de nube privada y proveedor de servicio en la nube, puedes usar un tejido protegido para proporcionar un entorno más seguro para las máquinas virtuales. Un tejido protegido consta de un Servicio de protección de host (HGS), por lo general, un clúster de tres nodos, además de uno o varios hosts protegidos y un conjunto de máquinas virtuales blindadas (VM).

> [!IMPORTANT]
> Asegúrese de que ha instalado la actualización acumulativa más reciente antes de implementar máquinas virtuales blindadas en producción.

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>Vídeos, blog e información general sobre tejidos protegidos y máquinas virtuales blindadas

- Video: [Cómo proteger el tejido de virtualización de amenazas internas con Windows Server 2019](https://myignite.techcommunity.microsoft.com/sessions/64690)
- Video: [Introducción a la Virtual Machines blindada en Windows Server 2016](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Video: [Profundice en máquinas virtuales blindadas con Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- Video: [Implementación de máquinas virtuales blindadas y un tejido protegido con Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- Blog: [Blog de seguridad de centro de seguridad y nube privada](https://blogs.technet.microsoft.com/datacentersecurity/)
- Información general: [Información general sobre máquinas virtuales blindadas y tejido protegido](Guarded-Fabric-and-Shielded-VMs.md)

## <a name="planning-topics"></a>Temas de planeación

- [Guía de planificación para proveedores de hospedaje](guarded-fabric-planning-for-hosters.md)
- [Guía de planeación para inquilinos](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>Temas de implementación

- [Guía de implementación](guarded-fabric-deploying-hgs-overview.md)
    - [Inicio rápido](guarded-fabric-deployment-overview.md)
    - [Implementar HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [Implementar hosts protegidos](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [Configuración del DNS de tejido para hosts que se convertirán en hosts protegidos](guarded-fabric-configuring-fabric-dns.md)
        - [Implementar un host protegido mediante el modo AD](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [Implementar un host protegido mediante el modo TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [Confirmar que los hosts protegidos pueden atestiguar](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [Máquinas virtuales blindadas: el proveedor de servicios de hosting implementa hosts protegidos en VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts)
    - [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [Creación de una plantilla de máquina virtual blindada](guarded-fabric-create-a-shielded-vm-template.md)
        - [Preparación de una aplicación auxiliar de blindaje de VM VHD](guarded-fabric-vm-shielding-helper-vhd.md)
        - [Instalar Windows Azure Pack](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [Creación de un archivo de datos de blindaje](guarded-fabric-tenant-creates-shielding-data.md)
        - [Implementación de una máquina virtual blindada mediante Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [Implementación de una máquina virtual blindada mediante Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>Tema sobre operaciones y administración

- [Administración del servicio de protección de host](guarded-fabric-manage-hgs.md)
