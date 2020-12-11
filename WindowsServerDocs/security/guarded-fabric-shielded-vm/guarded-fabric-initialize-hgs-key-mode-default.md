---
description: Más información acerca de cómo inicializar el clúster de HGS mediante el modo de clave en un nuevo bosque dedicado (valor predeterminado)
title: Inicializar el clúster de HGS mediante el modo de clave en un nuevo bosque dedicado (valor predeterminado)
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: b42535dbaa58b53e40aee555f1fdb47a24e94355
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049773"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-a-new-dedicated-forest-default"></a>Inicializar el clúster de HGS mediante el modo de clave en un nuevo bosque dedicado (valor predeterminado)

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016


1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Ejecute [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) en una ventana de PowerShell con privilegios elevados en el primer nodo de HGS. La sintaxis de este cmdlet admite muchas entradas diferentes, pero las dos invocaciones más comunes son las siguientes:

    -   Si usa archivos PFX para los certificados de cifrado y firma, ejecute los siguientes comandos:

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostkey
        ```

    -   Si usa certificados no exportables que están instalados en el almacén de certificados local, ejecute el siguiente comando. Si no conoce las huellas digitales de los certificados, puede hacer una lista de los certificados disponibles mediante la ejecución de `Get-ChildItem Cert:\LocalMachine\My` .

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustHostKey
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]


## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Crear clave de host](guarded-fabric-create-host-key.md)
