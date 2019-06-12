---
title: Inicializar el clúster HGS utilizando el modo de clave en un bosque bastión
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e785ee17bf68c07d965816480baa0d59062fc434
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447424"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-an-existing-bastion-forest"></a>Inicializar el clúster HGS utilizando el modo de clave en un bosque bastión existente

> Se aplica a: Windows Server 2019
> 
> [!div class="step-by-step"]
> [«Instalar HGS en un bosque nuevo](guarded-fabric-install-hgs-in-a-bastion-forest.md)
> [crear clave de host»](guarded-fabric-create-host-key.md)

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostKey
```

Si usa certificados instalados en el equipo local (como certificados respaldada por HSM y los certificados no exportable), use el `-SigningCertificateThumbprint` y `-EncryptionCertificateThumbprint` parámetros en su lugar.

