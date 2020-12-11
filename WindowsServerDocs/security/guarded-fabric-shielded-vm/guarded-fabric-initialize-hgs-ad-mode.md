---
description: Más información acerca de cómo inicializar HGS mediante la atestación de confianza de administrador
title: Inicializar HGS mediante la atestación de confianza de administrador
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 65ba0bd90d42ac038eeb8eb304def80ce7093ff1
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049763"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inicializar HGS mediante la atestación de confianza de administrador

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!IMPORTANT]
>La atestación de confianza de administrador (modo AD) está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar.


Estos pasos varían en función de si está inicializando HGS en un bosque nuevo o un bosque bastión existente:

1. [Inicializar el clúster de HGS en un nuevo bosque (valor predeterminado)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   O bien

   [Inicializar el clúster de HGS en un bosque bastión existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configuración del reenvío de DNS en el dominio de tejido](guarded-fabric-configuring-fabric-dns.md)

3. [Configuración del reenvío de DNS y una confianza unidireccional en el dominio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



