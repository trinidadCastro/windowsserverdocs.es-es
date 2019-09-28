---
title: Configuración del DNS de tejido para hosts protegidos (AD)
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 411b845d57c36916dcbc73d51675f5d9f92bfa0e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386757"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>Configuración del DNS de tejido para hosts protegidos

>Se aplica a: Windows Server 2016


>[!IMPORTANT]
>El modo de AD está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 

Un administrador de tejido debe configurar el tejido que DNS necesita para permitir que los hosts protegidos resuelvan el clúster de HGS. El clúster de HGS ya debe estar [configurado por el administrador de HGS](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Configurar DNS de HGS y una confianza unidireccional](guarded-fabric-configure-dns-forwarding-and-trust.md)
