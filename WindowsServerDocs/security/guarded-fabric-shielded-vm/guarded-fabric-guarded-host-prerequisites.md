---
title: Requisitos previos de host protegido
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5f2c3ec4b2c434ea945d86c4b1593e2e416a5123
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819236"
---
# <a name="prerequisites-for-guarded-hosts"></a>Requisitos previos para los hosts protegidos

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Revise los requisitos previos de host para el modo de atestación que haya elegido, a continuación, haga clic en el paso siguiente para agregar hosts protegidos.

## <a name="tpm-trusted-attestation"></a>Atestación de TPM de confianza

Los hosts protegidos utilizando el modo de TPM deben cumplir los siguientes requisitos previos:

-   **Hardware**: Un host es necesario para la implementación inicial. Para probar la migración en vivo de Hyper-V para máquinas virtuales blindadas, debe tener al menos dos hosts.

    Los hosts deben tener:
    
    - IOMMU y segundo traducción de direcciones de nivel (SLAT)
    - TPM 2.0
    - UEFI 2.3.1 o posterior
    - Configurado para arrancar con UEFI (no el BIOS o el modo "heredado")
    - Arranque seguro está habilitado
        
-   **Sistema operativo**: Windows Server 2016 Datacenter edition o posterior

    > [!IMPORTANT]
    > Asegúrese de instalar el [actualización acumulativa más reciente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).  

-   **Roles y características**: Rol de Hyper-V y la característica de compatibilidad de Hyper-V de guardián de Host. La característica de compatibilidad de Hyper-V de guardián de Host solo está disponible en las ediciones Datacenter de Windows Server. 

> [!WARNING]
> La característica de compatibilidad de Hyper-V de guardián de Host permite protección basada en virtualización de integridad de código que puede no ser compatible con algunos dispositivos. Se recomienda esta configuración de pruebas en su laboratorio antes de habilitar esta característica. Si no lo haces pueden producirse errores inesperados, incluida la pérdida de datos o un error de pantalla azul (también llamado error grave). Para obtener más información, consulte [hardware Compatible con la protección basada en Windows Server Virtualization de integridad de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Paso siguiente:** 
>[!div class="nextstepaction"]
[Capturar información TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>Atestación de clave de host

Los hosts protegidos con la atestación de clave de host deben cumplir los siguientes requisitos previos:

- **Hardware**: Cualquier servidor capaz de ejecutar el principio de Hyper-V con Windows Server 2019
- **Sistema operativo**: 2019 de Windows Server Datacenter edition
- **Roles y características**: Rol de Hyper-V y la característica de compatibilidad de Hyper-V de guardián de Host 

El host se puede unir a un dominio o un grupo de trabajo. 

Para la atestación de clave de host, HGS debe estar ejecutando Windows Server 2019 y funciona con la atestación de v2. Para obtener más información, consulte [requisitos previos HGS](guarded-fabric-prepare-for-hgs.md#prerequisites). 

**Paso siguiente:** 
>[!div class="nextstepaction"]
[Crear un par de claves](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>Atestación de administrador de confianza

>[!IMPORTANT]
>Atestación de administrador de confianza (modo de AD) está en desuso a partir de Windows Server 2019. Para entornos donde no es posible la atestación de TPM, configurar [atestación de clave de host](#host-key-attestation). Atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 

Hosts de Hyper-V deben cumplir los siguientes requisitos previos para el modo de AD:

-   **Hardware**: Cualquier servidor capaz de ejecutar el principio de Hyper-V con Windows Server 2016. Un host es necesario para la implementación inicial. Para probar la migración en vivo de Hyper-V para máquinas virtuales blindadas, necesita al menos dos hosts.

-   **Sistema operativo**: Windows Server 2016 Datacenter edition

    > [!IMPORTANT]
    > Instalar el [actualización acumulativa más reciente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).

-   **Roles y características**: Rol de Hyper-V y la característica de compatibilidad de Hyper-V de guardián de Host, que solo está disponible en Windows Server 2016 Datacenter edition. 

> [!WARNING]
> La característica de compatibilidad de Hyper-V de guardián de Host permite protección basada en virtualización de integridad de código que puede no ser compatible con algunos dispositivos. Se recomienda esta configuración de pruebas en su laboratorio antes de habilitar esta característica. Si no lo haces pueden producirse errores inesperados, incluida la pérdida de datos o un error de pantalla azul (también llamado error grave). Para obtener más información, consulte [hardware Compatible con la protección basada en virtualización de Windows Server 2016 de integridad de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Paso siguiente:** 
>[!div class="nextstepaction"]
[Colocar hosts protegidos en un grupo de seguridad](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)