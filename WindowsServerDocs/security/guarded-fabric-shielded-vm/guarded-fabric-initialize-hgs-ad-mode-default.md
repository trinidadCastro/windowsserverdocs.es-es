---
title: Inicializar el clúster HGS utilizando el modo de AD en un nuevo bosque dedicado (valor predeterminado)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7a3d38818bfdaa48f53ca7a54bf10b68e4e4a7d3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447442"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-a-new-dedicated-forest-default"></a>Inicializar el clúster HGS utilizando el modo de AD en un nuevo bosque dedicado (valor predeterminado)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!IMPORTANT]
>Atestación de administrador de confianza (modo de AD) está en desuso a partir de Windows Server 2019. Para entornos donde no es posible la atestación de TPM, configurar [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode-default.md). Atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Ejecute [Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx) en una ventana de PowerShell con privilegios elevados en el primer nodo HGS. La sintaxis de este cmdlet admite muchas entradas diferentes, pero las 2 invocaciones más comunes son a continuación:

    -   Si usa archivos PFX para los certificados de firma y cifrado, ejecute los siguientes comandos:

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
        ```

    -   Si usas no exportable certificados que están instalados en el almacén de certificados local, ejecute el siguiente comando. Si no conoce las huellas digitales de certificados, puede enumerar los certificados disponibles mediante la ejecución de `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustActiveDirectory
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]  

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Configurar el tejido DNS](guarded-fabric-configuring-fabric-dns-ad.md)