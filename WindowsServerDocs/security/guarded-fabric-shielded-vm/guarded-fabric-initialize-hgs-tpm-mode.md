---
description: Más información acerca de cómo inicializar HGS mediante la atestación de confianza de TPM
title: Inicializar HGS mediante la atestación de confianza de TPM
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 6dc4139b1204242a96b99e93d211a673756bdc5a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049743"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Inicializar HGS mediante la atestación de confianza de TPM

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Estos pasos varían en función de si está inicializando HGS en un bosque nuevo o un bosque bastión existente:

1. [Inicializar el clúster de HGS en un nuevo bosque (valor predeterminado)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   O bien

   [Inicializar el clúster de HGS en un bosque bastión existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Instalar certificados raíz de TPM de confianza](guarded-fabric-install-trusted-tpm-root-certificates.md)
3. [Configuración del DNS de tejido](guarded-fabric-configuring-fabric-dns.md)

