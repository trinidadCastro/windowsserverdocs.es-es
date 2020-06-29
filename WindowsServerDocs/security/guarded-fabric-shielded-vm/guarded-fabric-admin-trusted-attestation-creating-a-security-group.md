---
title: Crear un grupo de seguridad para los hosts protegidos y registrar el grupo con HGS
ms.prod: windows-server
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 84bf134ac5224f224339cc1ec216bded8cbcc792
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475682"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>Crear un grupo de seguridad para los hosts protegidos y registrar el grupo con HGS

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

> [!IMPORTANT]
> El modo de AD está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar.

En este tema se describen los pasos intermedios para preparar los hosts de Hyper-V para que se conviertan en hosts protegidos mediante la atestación de confianza de administrador (modo AD). Antes de llevar a cabo estos pasos, complete los pasos de [configuración del tejido DNS para hosts que se convertirán en hosts protegidos](guarded-fabric-configuring-fabric-dns-ad.md).


## <a name="create-a-security-group-and-add-hosts"></a>Creación de un grupo de seguridad y adición de hosts

1. Cree un nuevo grupo de seguridad **global** en el dominio de tejido y agregue hosts de Hyper-V que ejecutarán máquinas virtuales blindadas. Reinicie los hosts para actualizar la pertenencia a grupos.

2. Use Get-ADGroup para obtener el identificador de seguridad (SID) del grupo de seguridad y proporcionarlo al administrador de HGS.

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Comando Get-AdGroup con salida](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registro del SID del grupo de seguridad con HGS

1. En un servidor HGS, ejecute el siguiente comando para registrar el grupo de seguridad con HGS.
   Vuelva a ejecutar el comando si es necesario para los grupos adicionales.
   Proporcione un nombre descriptivo para el grupo.
   No es necesario que coincida con el nombre del grupo de seguridad de Active Directory.

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Para comprobar que se ha agregado el grupo, ejecute [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx).

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Confirmar la atestación](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="additional-references"></a>Referencias adicionales

- [Implementación del Servicio de protección de host para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-deploying-hgs-overview.md)
