---
title: Implementación de una infraestructura de red definida por software
description: En este tema se proporcionan vínculos a temas sobre cómo implementar una infraestructura de red definida por software (SDN) de Microsoft mediante scripts en Windows Server 2019 y 2016.
ms.topic: how-to
ms.assetid: 6c665c88-df28-4150-81d4-a47e9fa5255c
ms.date: 08/23/2018
ms.author: anpaul
author: AnirbanPaul
manager: grcusanz
ms.openlocfilehash: a9ce251488d80b4e5180d1641bc2157227d08d71
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716021"
---
# <a name="deploy-a-software-defined-network-infrastructure"></a>Implementar una infraestructura de red definida por software

>Se aplica a: Windows Server 2019, Windows Server 2016

Implementar la infraestructura de redes definidas por software (SDN) de Microsoft.

Estas implementaciones incluyen todas las tecnologías necesarias para una infraestructura totalmente funcional, como virtualización de red de Hyper-V (HNV), controladores de red, equilibradores de carga de software (SLB/MUX) y puertas de enlace.

## <a name="set-up-sdn-infrastructure-in-the-vmm-fabric"></a>Configuración de la infraestructura de SDN en el tejido de VMM




-   [Configurar una infraestructura de Redes definidas por software (SDN) en el tejido de VMM](/system-center/vmm/deploy-sdn)

    Use este método si desea incorporar System Center Virtual Machine Manager (VMM) para administrar la infraestructura de SDN.

## <a name="deploy-sdn-infrastructure-using-scripts"></a>Implementación de la infraestructura de SDN mediante scripts

-   [Implementación de una infraestructura de red definida por software con scripts](../../sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)

    Use este método si no desea usar VMM para administrar su infraestructura de SDN, o si tiene otro método de administración.


## <a name="deploy-individual-sdn-technologies-instead-of-an-entire-infrastructure"></a>Implementar tecnologías de SDN individuales en lugar de una infraestructura completa
 Si desea implementar tecnologías de SDN individuales en lugar de una infraestructura completa, consulte: [implementación de tecnologías de red definidas por software con Windows PowerShell](Deploy-Software-Defined-Network-Technologies-using-Windows-PowerShell.md).








## <a name="related-topics"></a>Temas relacionados
- [Redes definidas por software (SDN)](../software-defined-networking.md)
- [Tecnologías de SDN](../technologies/Software-Defined-Networking-Technologies.md)
- [Planeamiento de SDN](../plan/plan-a-software-defined-network-infrastructure.md)
- [Administrar SDN](../manage/manage-sdn.md)
- [Seguridad para SDN](../security/sdn-security-top.md)
- [Solucionar problemas de SDN](../troubleshoot/Troubleshoot-Software-Defined-Networking.md)