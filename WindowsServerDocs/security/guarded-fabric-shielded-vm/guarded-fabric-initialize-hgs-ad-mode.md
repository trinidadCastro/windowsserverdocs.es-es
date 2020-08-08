---
title: Inicializar HGS mediante la atestación de confianza de administrador
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: a3015c6af72b8a574ed1152198212b42618bb0fa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953471"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inicializar HGS mediante la atestación de confianza de administrador

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!IMPORTANT]
>La atestación de confianza de administrador (modo AD) está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar.


Estos pasos varían en función de si está inicializando HGS en un bosque nuevo o un bosque bastión existente:

1. [Inicializar el clúster de HGS en un nuevo bosque (valor predeterminado)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -O bien-

   [Inicializar el clúster de HGS en un bosque bastión existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configuración del reenvío de DNS en el dominio de tejido](guarded-fabric-configuring-fabric-dns.md)

3. [Configuración del reenvío de DNS y una confianza unidireccional en el dominio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



