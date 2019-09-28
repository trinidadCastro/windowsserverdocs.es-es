---
title: Inicializar HGS mediante la atestación de confianza de administrador
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 491754e8bcaad4524084604b78c7c6ed0fdee295
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402361"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inicializar HGS mediante la atestación de confianza de administrador

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!IMPORTANT]
>La atestación de confianza de administrador (modo AD) está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 


Estos pasos varían en función de si está inicializando HGS en un bosque nuevo o un bosque bastión existente:

1. [Inicializar el clúster de HGS en un nuevo bosque (valor predeterminado)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   O bien:

   [Inicializar el clúster de HGS en un bosque bastión existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configuración del reenvío de DNS en el dominio de tejido](guarded-fabric-configuring-fabric-dns.md)

3. [Configuración del reenvío de DNS y una confianza unidireccional en el dominio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



