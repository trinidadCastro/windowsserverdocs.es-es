---
title: Inicializar HGS con la atestación de administrador de confianza
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 664404cb72981e162bca016df14847e684d987c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821836"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inicializar HGS con la atestación de administrador de confianza

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!IMPORTANT]
>Atestación de administrador de confianza (modo de AD) está en desuso a partir de Windows Server 2019. Para entornos donde no es posible la atestación de TPM, configurar [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). Atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 


Estos pasos varían en función de si se están inicializando HGS en un bosque nuevo o un bosque bastión existente:

1. [Inicializar el clúster HGS en un bosque nuevo (valor predeterminado)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   O bien:

   [Inicializar el clúster HGS en un bosque bastión existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurar el reenvío de DNS en el dominio de fabric](guarded-fabric-configuring-fabric-dns.md)

3. [Configurar el reenvío de DNS y una confianza unidireccional en el dominio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



