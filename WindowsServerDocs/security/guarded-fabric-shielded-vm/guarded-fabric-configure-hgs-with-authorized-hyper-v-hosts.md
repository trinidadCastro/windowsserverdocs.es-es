---
title: Implementar hosts protegidos
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3b20a7eb2b5097d8ddb7381fd0304581ca4e6722
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845356"
---
# <a name="deploy-guarded-hosts"></a>Implementar hosts protegidos

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Los temas de esta sección describen los pasos que toma un administrador del tejido para configurar hosts de Hyper-V para que funcionen con el servicio de protección de Host (HGS). Antes de iniciar estos pasos, al menos un nodo en el [se debe configurar el clúster HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

**Para la atestación de TPM de confianza**:
1. [Configure el tejido DNS](guarded-fabric-configuring-fabric-dns.md): Indica cómo configurar un reenviador DNS del dominio de fabric para el dominio HGS.
2. [Capturar la información requerida por HGS](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): Indica cómo capturar los identificadores TPM (también denominados identificadores de la plataforma), crear una directiva de integridad de código y crear una instantánea TPM. A continuación, proporcionará esta información al administrador para configurar la atestación de HGS.
3. [Confirme que los hosts protegidos pueden dar fe](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Para la atestación de clave de host**:
1. [Crear una clave de host](guarded-fabric-create-host-key.md#create-a-host-key): Indica cómo configurar un reenviador DNS del dominio de fabric para el dominio HGS.
2. [Agregue la clave de host para el servicio de atestación](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): Indica cómo configurar un grupo de seguridad de Active Directory en el dominio de fabric, agregar los hosts protegidos como miembros de ese grupo y proporcione ese identificador de grupo para el Administrador de HGS. 
3. [Confirme que los hosts protegidos pueden dar fe](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Para la atestación de administrador de confianza**:
1. [Configure el tejido DNS](guarded-fabric-configuring-fabric-dns.md): Indica cómo configurar un reenviador DNS del dominio de fabric para el dominio HGS.
2. [Crear un grupo de seguridad](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): Indica cómo configurar un grupo de seguridad de Active Directory en el dominio de fabric, agregar los hosts protegidos como miembros de ese grupo y proporcione ese identificador de grupo para el Administrador de HGS. 
3. [Confirme que los hosts protegidos pueden dar fe](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Vea también

- [Tareas de implementación para protegidos tejidos y las máquinas virtuales blindadas](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
