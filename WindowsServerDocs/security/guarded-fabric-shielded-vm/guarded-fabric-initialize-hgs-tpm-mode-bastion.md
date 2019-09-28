---
title: Inicializar el clúster de HGS mediante el modo TPM en un bosque bastión
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b360f0f5195bea3c61f9a181b4b75a681f7e29b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403608"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-an-existing-bastion-forest"></a>Inicializar el clúster de HGS mediante el modo TPM en un bosque bastión existente

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
```

Si usa certificados instalados en el equipo local (por ejemplo, certificados respaldados por HSM y certificados no exportables), use en su lugar los parámetros `-SigningCertificateThumbprint` y `-EncryptionCertificateThumbprint`.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Instalar certificados raíz TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)