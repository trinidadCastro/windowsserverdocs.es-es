---
description: Más información acerca de cómo implementar hosts protegidos
title: Implementar hosts protegidos
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: fb3d189ce0817d52a536af8f25fd9b78fda719e5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049883"
---
# <a name="deploy-guarded-hosts"></a>Implementar hosts protegidos

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En los temas de esta sección se describen los pasos necesarios para que un administrador de tejido configure los hosts de Hyper-V para que funcionen con el servicio de protección de host (HGS). Antes de poder iniciar estos pasos, [debe configurarse](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)al menos un nodo en el clúster de HGS.

**Para la atestación de confianza de TPM**:
1. [Configuración del tejido DNS](guarded-fabric-configuring-fabric-dns.md): indica cómo configurar un reenviador de DNS desde el dominio del tejido al dominio HGS.
2. [Capturar información requerida por HGS](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): indica cómo capturar identificadores de TPM (también denominados identificadores de plataforma), crear una directiva de integridad de código y crear una línea base de TPM. A continuación, proporcionará esta información al administrador de HGS para configurar la atestación.
3. [Confirmar que los hosts protegidos pueden atestiguar](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Para la atestación de clave de host**:
1. [Crear una clave de host](guarded-fabric-create-host-key.md#create-a-host-key): indica cómo configurar un reenviador de DNS desde el dominio de tejido al dominio HGS.
2. [Agregar la clave de host al servicio de atestación](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): indica cómo configurar un grupo de seguridad de Active Directory en el dominio del tejido, agregar hosts protegidos como miembros de ese grupo y proporcionar ese identificador de grupo al administrador de HGS.
3. [Confirmar que los hosts protegidos pueden atestiguar](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Para la atestación de confianza de administrador**:
1. [Configuración del tejido DNS](guarded-fabric-configuring-fabric-dns.md): indica cómo configurar un reenviador de DNS desde el dominio del tejido al dominio HGS.
2. [Crear un grupo de seguridad](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): indica cómo configurar un grupo de seguridad de Active Directory en el dominio del tejido, agregar hosts protegidos como miembros de ese grupo y proporcionar ese identificador de grupo al administrador de HGS.
3. [Confirmar que los hosts protegidos pueden atestiguar](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="additional-references"></a>Referencias adicionales

- [Tareas de implementación de tejidos protegidos y máquinas virtuales blindadas](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
