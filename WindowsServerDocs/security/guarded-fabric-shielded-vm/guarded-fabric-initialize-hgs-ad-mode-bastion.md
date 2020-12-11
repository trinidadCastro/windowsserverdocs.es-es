---
description: Más información acerca de cómo inicializar el clúster HGS mediante el modo AD en un bosque bastión existente
title: Inicialización del clúster HGS mediante el modo AD en un bosque bastión
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: a37ad77686b9a75704045a28ae6cccc21dbe891b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049793"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>Inicializar el clúster HGS mediante el modo AD en un bosque bastión existente

>Se aplica a: Windows Server (canal semianual), Windows Server 2016


>[!IMPORTANT]
>La atestación de confianza de administrador (modo AD) está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode-bastion.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar.

Active Directory Domain Services se instalará en la máquina, pero debe permanecer sin configurar.

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

Antes de continuar, asegúrese de que ha preconfigurado los objetos de clúster para el servicio de protección de host y que ha concedido al usuario que ha iniciado sesión el **control total** sobre los objetos VCO y CNO en Active Directory.
El nombre de objeto de equipo virtual debe pasarse al `-HgsServiceName` parámetro y el nombre del clúster al `-ClusterName` parámetro.

> [!TIP]
> Compruebe los controladores de dominio de AD para asegurarse de que los objetos de clúster se han replicado en todos los controladores de dominio antes de continuar.

Si usa certificados basados en PFX, ejecute los siguientes comandos en el servidor HGS:

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

Si usa certificados instalados en el equipo local (por ejemplo, certificados respaldados por HSM y certificados no exportables), use los `-SigningCertificateThumbprint` `-EncryptionCertificateThumbprint` parámetros y en su lugar.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Configurar el tejido DNS](guarded-fabric-configuring-fabric-dns-ad.md)

