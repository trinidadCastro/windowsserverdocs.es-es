---
title: Implementación del servicio de protección de host
ms.prod: windows-server
ms.topic: article
ms.assetid: 310b63d9-5ac7-4961-98ef-103af45d706a
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: c1eea8c7f6da1140480d0a8deaafb2edb73528de
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769153"
---
# <a name="deploying-the-host-guardian-service"></a>Implementación del servicio de protección de host

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Uno de los objetivos más importantes de proporcionar un entorno hospedado es garantizar la seguridad de las máquinas virtuales que se ejecutan en el entorno. Como administrador de empresa de nube privada y proveedor de servicio en la nube, puedes usar un tejido protegido para proporcionar un entorno más seguro para las máquinas virtuales. Un tejido protegido consta de un servicio de protección de host (HGS), por lo general, un clúster de tres nodos, además de uno o varios hosts protegidos y un conjunto de máquinas virtuales blindadas (VM).

## <a name="video-deploying-a-guarded-fabric"></a>Vídeo: implementación de un tejido protegido

> [!VIDEO https://www.microsoft.com/videoplayer/embed/dcd8e99f-36f1-4bc8-b3d2-9576da38d9f1?autoplay=false]

## <a name="deployment-tasks-for-guarded-fabrics-and-shielded-vms"></a>Tareas de implementación de tejidos protegidos y máquinas virtuales blindadas

En la tabla siguiente se desglosan las tareas para implementar un tejido protegido y crear máquinas virtuales blindadas según los distintos roles de administrador. Tenga en cuenta que cuando el administrador de HGS configura HGS con hosts de Hyper-V autorizados, un administrador de tejido recopilará y proporcionará información de identificación sobre los hosts al mismo tiempo.

| Paso y vínculo a contenido | Imagen |
|--|--|--|
| 1- [comprobación de los requisitos previos de HGS](guarded-fabric-prepare-for-hgs.md) | ![Paso 1, comprobar los requisitos previos](../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png) |
| 2: [configuración del primer nodo HGS](guarded-fabric-choose-where-to-install-hgs.md) | ![Paso 2, configurar el primer nodo de HGS](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-first-hgs-node.png) |
| 3- [configuración de nodos de HGS adicionales](guarded-fabric-configure-additional-hgs-nodes.md) | ![Paso 3, configuración de nodos de HGS adicionales](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-secondary-hgs-nodes.png) |
| 4: [configuración de DNS de tejido](guarded-fabric-configuring-fabric-dns.md) | ![Paso 4, configuración de DNS de tejido](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-fabric-dns.png) |
| 5- [comprobación de los requisitos previos de host (clave)](guarded-fabric-guarded-host-prerequisites.md#host-key-attestation) y comprobación de los [requisitos previos de host (TPM)](guarded-fabric-guarded-host-prerequisites.md#tpm-trusted-attestation) | ![Paso 5, comprobar la clave del requisito previo del host y el TPM del requisito previo del host](../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png) |
| 6- [crear clave de host (clave)](guarded-fabric-create-host-key.md) y[recopilar información de host (TPM)](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md) | ![Paso 6, creación de la clave de host y recopilación de información de host](../media/Guarded-Fabric-Shielded-VM/guarded-host-collect-info-from-hosts.png) |
| 7- [configuración de HGS con información de host](guarded-fabric-add-host-information-to-hgs.md) | ![Paso 7, agregar información de host a HGS](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-hgs-with-host-info.png) |
| 8- [confirmar que los hosts pueden atestiguar](guarded-fabric-confirm-hosts-can-attest-successfully.md) | ![Paso 8, confirmar que el host puede atestiguar](../media/Guarded-Fabric-Shielded-VM/guarded-host-confirm-hosts-attest.png) |
| 9- [configuración de VMM (opcional)](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview) | ![Paso 9, configuración de VMM (opcional)](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-vmm.png) |
| 10- [creación de discos de plantilla](guarded-fabric-create-a-shielded-vm-template.md) | ![Paso 10, creación de discos de plantilla](../media/Guarded-Fabric-Shielded-VM/guarded-host-create-template-disk.png) |
| 11- [creación de un disco auxiliar de blindaje de máquina virtual para VMM (opcional)](guarded-fabric-vm-shielding-helper-vhd.md) | ![Paso 11, creación de un disco de ayuda de blindaje de máquina virtual para VMM](../media/Guarded-Fabric-Shielded-VM/guarded-host-create-helper-disk.png) |
| 12- [configurar Windows Azure Pack (opcional)](guarded-fabric-shielded-vm-windows-azure-pack.md) | ![Paso 12, configuración de Windows Azure Pack (opcional)](../media/Guarded-Fabric-Shielded-VM/guarded-host-windows-azure-pack.png) |
| 13- [creación](guarded-fabric-tenant-creates-shielding-data.md) de un archivo de datos de blindaje | ![Paso 13, creación de un archivo de datos de blindaje](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-file.png) |
| 14- [creación de máquinas virtuales blindadas mediante Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md) | ![Paso 14, creación de máquinas virtuales blindadas mediante Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png) |
| 15- [creación de máquinas virtuales blindadas con VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-vms) | ![Paso 15, creación de máquinas virtuales blindadas con VMM](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png) |

## <a name="additional-references"></a>Referencias adicionales

- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
