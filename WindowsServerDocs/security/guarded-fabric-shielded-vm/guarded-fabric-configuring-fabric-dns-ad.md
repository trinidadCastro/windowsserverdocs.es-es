---
title: Configuración del DNS de tejido para hosts protegidos (AD)
ms.prod: windows-server
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 208fe5c3919d61e0d00be9ccf2f5ae86eaa5ea8d
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769663"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts-ad"></a>Configuración del DNS de tejido para hosts protegidos (AD)

>Se aplica a: Windows Server 2016


>[!IMPORTANT]
>El modo de AD está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar.

Un administrador de tejido debe configurar el tejido que DNS necesita para permitir que los hosts protegidos resuelvan el clúster de HGS.
El clúster de HGS ya debe estar [configurado por el administrador de HGS](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)]


## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Configurar DNS de HGS y una confianza unidireccional](guarded-fabric-configure-dns-forwarding-and-trust.md)
