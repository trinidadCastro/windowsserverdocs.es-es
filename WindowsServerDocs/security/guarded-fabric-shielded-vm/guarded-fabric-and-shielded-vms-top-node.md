---
title: Máquinas virtuales blindadas y tejido protegido
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c2c16574439569eeb1181977f23bae238f43865c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857436"
---
# <a name="guarded-fabric-and-shielded-vms"></a>Máquinas virtuales blindadas y tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Uno de los objetivos más importantes de proporcionar un entorno hospedado es garantizar la seguridad de las máquinas virtuales que se ejecutan en el entorno. Como administrador de empresa de nube privada y proveedor de servicio en la nube, puedes usar un tejido protegido para proporcionar un entorno más seguro para las máquinas virtuales. Un tejido protegido consta de un Servicio de protección de host (HGS), por lo general, un clúster de tres nodos, además de uno o varios hosts protegidos y un conjunto de máquinas virtuales blindadas (VM).

> [!IMPORTANT]
> Asegúrese de que ha instalado la actualización acumulativa más reciente antes de implementar máquinas virtuales blindadas en producción.

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>Tema de introducción, blog y vídeos aproximadamente protegidos tejidos y las máquinas virtuales blindadas

- Video: [Cómo proteger al tejido de virtualización de las amenazas internas con Windows Server 2019](https://myignite.techcommunity.microsoft.com/sessions/64690)
- Video: [Introducción a las máquinas virtuales blindadas en Windows Server 2016](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Video: [Profundice en las máquinas virtuales blindadas con Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- Video: [Implementación de máquinas virtuales blindadas y un tejido protegido con Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- Blog: [Blog de seguridad de la nube privada y centro de datos](https://blogs.technet.microsoft.com/datacentersecurity/)
- Información general: [Información general de las máquinas virtuales blindada y tejido protegido](Guarded-Fabric-and-Shielded-VMs.md)

## <a name="planning-topics"></a>Temas de planificación

- [Guía de planeación para los proveedores de hospedaje](guarded-fabric-planning-for-hosters.md)
- [Guía de planeación para los inquilinos](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>Temas de implementación

- [Guía de implementación](guarded-fabric-deploying-hgs-overview.md)
    - [Guía de inicio rápido](guarded-fabric-deployment-overview.md)
    - [Implementar HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [Implementar hosts protegidos](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [Configurar al tejido de DNS para los hosts que se convertirá en hosts protegidos](guarded-fabric-configuring-fabric-dns.md)
        - [Implementar un host protegido con el modo de AD](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [Implementar un host protegido con el modo TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [Confirme que los hosts protegidos pueden dar fe](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [Proveedor de servicios de hospedaje de máquinas virtuales blindadas: implementa los hosts protegidos en VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts)
    - [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [Crear una plantilla de máquina virtual blindada](guarded-fabric-create-a-shielded-vm-template.md)
        - [Preparar una aplicación auxiliar de blindaje VHD](guarded-fabric-vm-shielding-helper-vhd.md)
        - [Configurar Windows Azure Pack](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [Crear un archivo de datos de blindaje](guarded-fabric-tenant-creates-shielding-data.md)
        - [Implementar una máquina virtual blindada mediante Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [Implementar una máquina virtual blindada mediante el uso de Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>Las operaciones y el tema de administración

- [Administrar el servicio de protección de Host](guarded-fabric-manage-hgs.md)
