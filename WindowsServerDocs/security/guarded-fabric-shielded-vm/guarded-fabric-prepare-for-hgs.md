---
description: 'Más información sobre: revisión de los requisitos previos para el servicio de protección de host'
title: Revisar los requisitos previos de HGS
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: a5c551718afc5578432f5a32c12b04ea12e4d051
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043913"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>Revisar los requisitos previos para el servicio de protección de host

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


En este tema se tratan los requisitos previos de HGS y los pasos iniciales para preparar la implementación de HGS.

## <a name="prerequisites"></a>Prerrequisitos

-   **Hardware**: HGS puede ejecutarse en máquinas físicas o virtuales, pero se recomiendan máquinas físicas.

    Si desea ejecutar HGS como un clúster físico de tres nodos (para disponibilidad), debe tener tres servidores físicos. (Como procedimiento recomendado para la agrupación en clústeres, los tres servidores deben tener hardware muy similar).

-   **Sistema operativo**: la atestación de clave de host requiere Windows Server 2019 Standard o Datacenter Edition que funcione con la [atestación V2](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies). En el caso de la atestación basada en TPM, HGS puede ejecutar Windows Server 2019 o Windows Server 2016, Standard o Datacenter Edition.

-   **Roles de servidor**: servicio de protección de host y roles de servidor de soporte.

-   **Permisos y privilegios de configuración para el dominio de tejido (host)**: deberá configurar el reenvío de DNS entre el dominio de tejido (host) y el dominio de HGS.

## <a name="upgrading-hgs"></a>Actualización de HGS

Si ya ha implementado HGS y quiere actualizar su sistema operativo, siga las [instrucciones de actualización](guarded-fabric-upgrade-to-2019.md) para actualizar los servidores HGS y Hyper-V al sistema operativo más reciente.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Obtener certificados para HGS](guarded-fabric-obtain-certs.md)
