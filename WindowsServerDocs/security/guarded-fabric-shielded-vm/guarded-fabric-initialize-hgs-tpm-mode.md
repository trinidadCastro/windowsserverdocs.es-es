---
title: Inicializar HGS mediante la atestación de confianza de TPM
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b8ceebe63a586ec95b502dfea12f99d174549448
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856618"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Inicializar HGS mediante la atestación de confianza de TPM

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Estos pasos varían en función de si está inicializando HGS en un bosque nuevo o un bosque bastión existente:

1. [Inicializar el clúster de HGS en un nuevo bosque (valor predeterminado)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   O bien:

   [Inicializar el clúster de HGS en un bosque bastión existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Instalar certificados raíz de TPM de confianza](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configuración del DNS de tejido](guarded-fabric-configuring-fabric-dns.md)

