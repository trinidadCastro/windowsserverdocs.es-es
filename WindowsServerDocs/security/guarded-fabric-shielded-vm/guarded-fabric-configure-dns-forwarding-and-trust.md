---
title: Configurar el reenvío de DNS y de confianza de dominio
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3f9083d749ba9251ba47ecb64b7cb3d7c6290f1d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443771"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>Configurar el reenvío de DNS en el dominio HGS y una confianza unidireccional con el dominio de fabric

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!IMPORTANT]
>Modo de AD está en desuso a partir de Windows Server 2019. Para entornos donde no es posible la atestación de TPM, configurar [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). Atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 

Utilice los pasos siguientes para configurar el reenvío de DNS y establecer una confianza unidireccional con el dominio de fabric. Estos pasos permiten el HGS para localizar al tejido de controladores de dominio y validación la pertenencia a grupos de los hosts de Hyper-V.

1.  Ejecute el siguiente comando en una sesión de PowerShell con privilegios elevados para configurar el reenvío de DNS. Reemplace fabrikam.com por el nombre del dominio de fabric y escriba las direcciones IP de los servidores DNS en el dominio de fabric. Para una mayor disponibilidad, apuntar a más de un servidor DNS.

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  Para crear una confianza de bosque unidireccional, ejecute el siguiente comando en un símbolo del sistema con privilegios elevados:

    Reemplace `bastion.local` con el nombre del dominio HGS y `fabrikam.com` con el nombre del dominio de fabric. Proporcione la contraseña de administrador del dominio de fabric.

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>Paso siguiente 

> [!div class="nextstepaction"]
> [Configurar HTTPS](guarded-fabric-configure-hgs-https.md)
