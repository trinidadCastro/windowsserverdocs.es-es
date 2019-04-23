---
title: Revisar los requisitos previos HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: dddf694aaceab93bd102456dbe86df17a001cb01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879886"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>Revisar los requisitos previos para el servicio de protección de Host

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


En este tema se trata los requisitos previos HGS y pasos iniciales para preparar la implementación de HGS.

## <a name="prerequisites"></a>Requisitos previos 

-   **Hardware**: HGS se pueden ejecutar en máquinas físicas o virtuales, pero se recomiendan máquinas físicas.

    Si desea ejecutar HGS como un clúster de tres nodos físico (para disponibilidad), debe tener tres servidores físicos. (Como una práctica recomendada para la agrupación en clústeres, los tres servidores deben tener hardware muy similar).
  
-   **Sistema operativo**: Atestación de clave de host requiere Windows Server 2019 Standard o Datacenter edition funciona con [v2 atestación](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies). Para la atestación basada en TPM, puede ejecutar HGS 2019 de Windows Server o Windows Server 2016, Standard o Datacenter edition.

-   **Roles de servidor**: Servicio guardián de host y compatibles con los roles de servidor.

-   **Permisos y privilegios de configuración para el dominio de tejido (host)**: Deberá configurar el reenvío de DNS entre el dominio de tejido (host) y el dominio HGS. 
    
## <a name="upgrading-hgs"></a>Actualización de HGS

Si ya ha implementado HGS y desea actualizar su sistema operativo, siga el [actualizar orientación](guarded-fabric-upgrade-to-2019.md) para actualizar los servidores de Hyper-V y HGS para el sistema operativo más reciente.

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Obtener certificados de HGS](guarded-fabric-obtain-certs.md)