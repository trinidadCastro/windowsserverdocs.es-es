---
title: Crear un grupo de seguridad para hosts protegidos y registrar el grupo con HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ba547dff862a283b68ff105a14b14ed367891745
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816346"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>Crear un grupo de seguridad para hosts protegidos y registrar el grupo con HGS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!IMPORTANT]
>Modo de AD está en desuso a partir de Windows Server 2019. Para entornos donde no es posible la atestación de TPM, configurar [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). Atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 


En este tema se describe los pasos intermedios para preparar los hosts de Hyper-V para convertirse en hosts protegidos con la atestación de administrador de confianza (modo de AD). Antes de realizar estos pasos, complete los pasos de [configurar el tejido de DNS para los hosts que se convertirá en hosts protegidos](guarded-fabric-configuring-fabric-dns-ad.md).


## <a name="create-a-security-group-and-add-hosts"></a>Cree un grupo de seguridad y agréguele hosts

1. Cree un nuevo **GLOBAL** seguridad de grupo en el dominio de tejido y agregar hosts de Hyper-V que se ejecutarán las máquinas virtuales blindadas. Reinicie los hosts para actualizar su pertenencia a grupos.

2. Use Get-ADGroup para obtener el identificador de seguridad (SID) del grupo de seguridad y proporcionarla al administrador de HGS. 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Comando Get-AdGroup con salida](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrar al SID del grupo de seguridad con HGS  

1. En un servidor HGS, ejecute el siguiente comando para registrar el grupo de seguridad con HGS. 
   Vuelva a ejecutar el comando si es necesario para los grupos adicionales. 
   Proporcione un nombre descriptivo para el grupo. 
   No es necesario para que coincida con el nombre del grupo de seguridad de Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Para comprobar que se agregó el grupo, ejecute [Get HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Confirmar atestación](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Vea también

- [Implementar el servicio de protección de Host para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-deploying-hgs-overview.md)
