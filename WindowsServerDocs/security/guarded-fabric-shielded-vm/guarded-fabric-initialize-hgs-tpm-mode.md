---
title: Inicializar HGS con la atestación de TPM de confianza
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 672054462437b615236dfc4f00c8b20c946f11a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882336"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Inicializar HGS con la atestación de TPM de confianza

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Estos pasos varían en función de si se están inicializando HGS en un bosque nuevo o un bosque bastión existente:

1. [Inicializar el clúster HGS en un bosque nuevo (valor predeterminado)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   O bien:

   [Inicializar el clúster HGS en un bosque bastión existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Instalar certificados de raíz de confianza de TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configurar al tejido de DNS](guarded-fabric-configuring-fabric-dns.md)

