---
title: Inicializar el clúster de HGS mediante el modo de clave en un bosque bastión
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 45f0f587c17e90251da2632f034fe03e2dc1c083
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856658"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-an-existing-bastion-forest"></a>Inicializar el clúster de HGS mediante el modo de clave en un bosque bastión existente

> Se aplica a: Windows Server 2019
> 
> [!div class="step-by-step"]
> [«Instalar HGS en un nuevo bosque](guarded-fabric-install-hgs-in-a-bastion-forest.md)
> [crear clave de host»](guarded-fabric-create-host-key.md)

Active Directory Domain Services se instalará en la máquina, pero debe permanecer sin configurar.

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)] 

Antes de continuar, asegúrese de que ha preconfigurado los objetos de clúster para el servicio de protección de host y que ha concedido al usuario que ha iniciado sesión el **control total** sobre los objetos VCO y CNO en Active Directory.
El nombre de objeto de equipo virtual debe pasarse al parámetro `-HgsServiceName` y el nombre del clúster al parámetro `-ClusterName`.

> [!TIP]
> Compruebe los controladores de dominio de AD para asegurarse de que los objetos de clúster se han replicado en todos los controladores de dominio antes de continuar.

Si usa certificados basados en PFX, ejecute los siguientes comandos en el servidor HGS:

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostKey
```

Si usa certificados instalados en el equipo local (como certificados respaldados por HSM y certificados no exportables), use los parámetros `-SigningCertificateThumbprint` y `-EncryptionCertificateThumbprint` en su lugar.

