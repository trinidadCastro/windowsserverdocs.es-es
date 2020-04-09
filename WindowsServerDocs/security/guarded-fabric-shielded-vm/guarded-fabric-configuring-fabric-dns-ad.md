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
ms.openlocfilehash: 5b7deafb49083ad55d5a49d1ad3c5e063e91483f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856828"
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
