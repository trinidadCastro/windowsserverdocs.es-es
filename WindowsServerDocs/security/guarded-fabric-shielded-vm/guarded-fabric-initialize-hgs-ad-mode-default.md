---
description: Más información acerca de cómo inicializar el clúster HGS mediante el modo AD en un nuevo bosque dedicado (valor predeterminado)
title: Inicialización del clúster HGS mediante el modo AD en un nuevo bosque dedicado (valor predeterminado)
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: a477ae4010c201b5dec89f5c3d938fd25991245d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047373"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-a-new-dedicated-forest-default"></a>Inicialización del clúster HGS mediante el modo AD en un nuevo bosque dedicado (valor predeterminado)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!IMPORTANT]
>La atestación de confianza de administrador (modo AD) está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode-default.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar.

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Ejecute [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) en una ventana de PowerShell con privilegios elevados en el primer nodo de HGS. La sintaxis de este cmdlet admite muchas entradas diferentes, pero las dos invocaciones más comunes son las siguientes:

    -   Si usa archivos PFX para los certificados de cifrado y firma, ejecute los siguientes comandos:

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
        ```

    -   Si usa certificados no exportables que están instalados en el almacén de certificados local, ejecute el siguiente comando. Si no conoce las huellas digitales de los certificados, puede hacer una lista de los certificados disponibles mediante la ejecución de `Get-ChildItem Cert:\LocalMachine\My` .

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustActiveDirectory
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Configurar el tejido DNS](guarded-fabric-configuring-fabric-dns-ad.md)
