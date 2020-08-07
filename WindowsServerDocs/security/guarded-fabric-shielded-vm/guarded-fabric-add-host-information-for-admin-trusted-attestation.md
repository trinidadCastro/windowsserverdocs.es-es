---
title: Agregar información de host para la atestación de confianza de administrador
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: abc01dbb691843d199169bd654afaa5cf06bbb87
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971442"
---
# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>Autorizar a los hosts de Hyper-V mediante la atestación de confianza de administrador

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

> [!IMPORTANT]
> La atestación de confianza de administrador (modo AD) está en desuso a partir de Windows Server 2019. En entornos donde no es posible la atestación de TPM, configure la [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). La atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar.


Para autorizar un host protegido en el modo AD:

1. En el dominio del tejido, agregue los hosts de Hyper-V a un grupo de seguridad.
2. En el dominio HGS, registre el SID del grupo de seguridad con HGS.

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>Agregar el host de Hyper-V a un grupo de seguridad y reiniciar el host

1. Cree un grupo de seguridad **global** en el dominio de tejido y agregue hosts de Hyper-V que ejecutarán máquinas virtuales blindadas.
   Reinicie los hosts para actualizar la pertenencia a grupos.

2. Use Get-ADGroup para obtener el identificador de seguridad (SID) del grupo de seguridad y proporcionarlo al administrador de HGS.

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![Comando Get-AdGroup con salida](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registro del SID del grupo de seguridad con HGS

1. Obtenga el SID del grupo de seguridad para los hosts protegidos del administrador del tejido y ejecute el siguiente comando para registrar el grupo de seguridad con HGS.
   Vuelva a ejecutar el comando si es necesario para los grupos adicionales.
   Proporcione un nombre descriptivo para el grupo.
   No es necesario que coincida con el nombre del grupo de seguridad de Active Directory.

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Para comprobar que se ha agregado el grupo, ejecute [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx).


