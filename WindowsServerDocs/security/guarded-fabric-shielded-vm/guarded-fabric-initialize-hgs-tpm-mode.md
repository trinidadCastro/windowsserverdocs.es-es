---
title: Inicializar HGS mediante la atestación de confianza de TPM
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c67e15aa011dbff0be5d428c8012a161d9eaea2f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386596"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Inicializar HGS mediante la atestación de confianza de TPM

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Estos pasos varían en función de si está inicializando HGS en un bosque nuevo o un bosque bastión existente:

1. [Inicializar el clúster de HGS en un nuevo bosque (valor predeterminado)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   O bien:

   [Inicializar el clúster de HGS en un bosque bastión existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Instalar certificados raíz de TPM de confianza](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configuración del DNS de tejido](guarded-fabric-configuring-fabric-dns.md)

