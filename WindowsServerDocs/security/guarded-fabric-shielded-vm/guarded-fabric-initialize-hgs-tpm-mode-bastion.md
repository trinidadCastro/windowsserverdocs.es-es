---
title: Inicializar el clúster HGS utilizando el modo TPM en un bosque bastión
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ac6407f9f23780dc62ff7d99fe0daf0f81f79f72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832146"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-an-existing-bastion-forest"></a>Inicializar el clúster HGS utilizando el modo TPM en un bosque bastión existente

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Servicios de dominio de Active Directory se instalará en el equipo, pero deben permanecer sin configurar.

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

Antes de continuar, asegúrese de que ha preconfigurado a los objetos de clúster para el servicio de protección de Host y conceder el inicio de sesión de usuario **Control total** a través de los objetos VCO y CNO en Active Directory.
El nombre del objeto de equipo virtual debe pasarse a la `-HgsServiceName` parámetro y el nombre del clúster para el `-ClusterName` parámetro.

> [!TIP]
> Vuelva a comprobar los controladores de dominio de Active Directory para asegurarse de los objetos de clúster han replicado a todos los controladores de dominio antes de continuar.

Si usa certificados basados en PFX, ejecute los siguientes comandos en el servidor HGS:

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
```

Si usa certificados instalados en el equipo local (como certificados respaldada por HSM y los certificados no exportable), use el `-SigningCertificateThumbprint` y `-EncryptionCertificateThumbprint` parámetros en su lugar.

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Instalar certificados raíz TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)