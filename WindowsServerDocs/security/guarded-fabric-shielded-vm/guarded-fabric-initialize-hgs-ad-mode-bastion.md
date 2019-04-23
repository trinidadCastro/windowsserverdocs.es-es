---
title: Inicializar el clúster HGS utilizando el modo de AD en un bosque bastión
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 98003745823cf780a38487dff997798ebef12fc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870096"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>Inicializar el clúster HGS utilizando el modo de AD en un bosque bastión existente

>Se aplica a: Windows Server (canal semianual), Windows Server 2016


>[!IMPORTANT]
>Atestación de administrador de confianza (modo de AD) está en desuso a partir de Windows Server 2019. Para entornos donde no es posible la atestación de TPM, configurar [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode-bastion.md). Atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

Si usa certificados instalados en el equipo local (como certificados respaldada por HSM y los certificados no exportable), use el `-SigningCertificateThumbprint` y `-EncryptionCertificateThumbprint` parámetros en su lugar.

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Configurar el tejido de DNS](guarded-fabric-configuring-fabric-dns-ad.md)

