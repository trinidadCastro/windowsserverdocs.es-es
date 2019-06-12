---
title: Configurar al tejido de DNS para hosts protegidos (AD)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9d302dcd06b7a3a40afbb6f613c39caaabbeba91
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443720"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>Configurar al tejido de DNS para hosts protegidos

>Se aplica a: Windows Server 2016


>[!IMPORTANT]
>Modo de AD está en desuso a partir de Windows Server 2019. Para entornos donde no es posible la atestación de TPM, configurar [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). Atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 

Un administrador del tejido debe configurar al tejido de que DNS tarda en permitir que los hosts protegidos resolver el clúster HGS. Ya debe estar el clúster HGS [establecidas por el administrador HGS](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Configurar DNS de HGS y una confianza unidireccional](guarded-fabric-configure-dns-forwarding-and-trust.md)
