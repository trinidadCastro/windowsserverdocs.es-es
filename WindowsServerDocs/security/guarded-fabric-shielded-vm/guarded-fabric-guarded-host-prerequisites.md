---
title: Requisitos previos de host protegido
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8a9273eef906130b11b98148cf1e84f7e18812b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402380"
---
# <a name="prerequisites-for-guarded-hosts"></a>Requisitos previos de los hosts protegidos

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Revise los requisitos previos de host para el modo de atestación elegido y, a continuación, haga clic en el paso siguiente para agregar los hosts protegidos.

## <a name="tpm-trusted-attestation"></a>Atestación de confianza de TPM

Los hosts protegidos que usan el modo TPM deben cumplir los siguientes requisitos previos:

-   **Hardware**: Se requiere un host para la implementación inicial. Para probar la migración en vivo de Hyper-V para máquinas virtuales blindadas, debe tener al menos dos hosts.

    Los hosts deben tener:
    
    - Traducción de direcciones de segundo nivel y IOMMU (SLAT)
    - TPM 2.0
    - UEFI 2.3.1 o posterior
    - Configurado para el arranque con UEFI (no BIOS o modo "heredado")
    - Arranque seguro habilitado
        
-   **Sistema operativo**: Windows Server 2016 Datacenter Edition o posterior

    > [!IMPORTANT]
    > Asegúrese de instalar la [actualización acumulativa más reciente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).  

-   **Rol y características**: Rol de Hyper-V y la característica de compatibilidad de Hyper-V de protección de host. La característica de compatibilidad de Hyper-V de protección de host solo está disponible en las ediciones Datacenter de Windows Server. 

> [!WARNING]
> La característica de compatibilidad de Hyper-V de protección de host permite la protección basada en la virtualización de la integridad de código que puede ser incompatible con algunos dispositivos. Se recomienda encarecidamente probar esta configuración en el laboratorio antes de habilitar esta característica. Si no lo haces pueden producirse errores inesperados, incluida la pérdida de datos o un error de pantalla azul (también llamado error grave). Para obtener más información, vea [hardware compatible con la protección basada en la virtualización de Windows Server de la integridad de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Siguiente paso:** 
> [!div class="nextstepaction"]
> [Capturar información de TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>Atestación de clave de host

Los hosts protegidos que usan la atestación de clave de host deben cumplir los siguientes requisitos previos:

- **Hardware**: Cualquier servidor capaz de ejecutar Hyper-V a partir de Windows Server 2019
- **Sistema operativo**: Windows Server 2019 Datacenter Edition
- **Rol y características**: Rol de Hyper-V y la característica de compatibilidad de Hyper-V de protección de host 

El host se puede unir a un dominio o a un grupo de trabajo. 

En el caso de la atestación de clave de host, HGS debe ejecutar Windows Server 2019 y funcionar con la atestación V2. Para obtener más información, consulte [requisitos previos de HGS](guarded-fabric-prepare-for-hgs.md#prerequisites). 

**Siguiente paso:** 
> [!div class="nextstepaction"]
> [Crear un par de claves](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>Atestación de confianza de administrador

>[!IMPORTANT]
>La atestación de confianza de administrador (modo AD) está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](#host-key-attestation). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 

Los hosts de Hyper-V deben cumplir los siguientes requisitos previos para el modo AD:

-   **Hardware**: Cualquier servidor capaz de ejecutar Hyper-V a partir de Windows Server 2016. Se requiere un host para la implementación inicial. Para probar la migración en vivo de Hyper-V para máquinas virtuales blindadas, necesita al menos dos hosts.

-   **Sistema operativo**: Windows Server 2016 Datacenter Edition

    > [!IMPORTANT]
    > Instale la [actualización acumulativa más reciente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).

-   **Rol y características**: Rol de Hyper-V y la característica de compatibilidad de Hyper-V de protección de host, que solo está disponible en Windows Server 2016 Datacenter Edition. 

> [!WARNING]
> La característica de compatibilidad de Hyper-V de protección de host permite la protección basada en la virtualización de la integridad de código que puede ser incompatible con algunos dispositivos. Se recomienda encarecidamente probar esta configuración en el laboratorio antes de habilitar esta característica. Si no lo haces pueden producirse errores inesperados, incluida la pérdida de datos o un error de pantalla azul (también llamado error grave). Para obtener más información, consulte [hardware compatible con la protección basada en la virtualización de Windows Server 2016 de la integridad de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Siguiente paso:** 
> [!div class="nextstepaction"]
> [Colocar hosts protegidos en un grupo de seguridad](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)