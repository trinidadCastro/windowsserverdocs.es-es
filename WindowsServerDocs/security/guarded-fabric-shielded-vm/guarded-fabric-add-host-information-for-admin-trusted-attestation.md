---
title: Agregue información del host de atestación de administrador de confianza
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7949711dbb0f89f5404b491d60938985bfa98c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849466"
---
>Se aplica a: Windows Server (canal semianual), Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>Autorizar a los hosts de Hyper-V con la atestación de administrador de confianza

>[!IMPORTANT]
>Atestación de administrador de confianza (modo de AD) está en desuso a partir de Windows Server 2019. Para entornos donde no es posible la atestación de TPM, configurar [atestación de clave de host](guarded-fabric-initialize-hgs-key-mode.md). Atestación de clave de host proporciona una garantía similar al modo de AD y es más fácil de configurar. 


Para autorizar a un host protegido en el modo de AD: 

1. En el dominio de fabric, agregar los hosts de Hyper-V a un grupo de seguridad.
2. En el dominio HGS, registre al SID del grupo de seguridad con HGS. 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>Agregar el host de Hyper-V a un grupo de seguridad y reinicie el host

1. Crear un **GLOBAL** seguridad de grupo en el dominio de tejido y agregar hosts de Hyper-V que se ejecutarán las máquinas virtuales blindadas. 
   Reinicie los hosts para actualizar su pertenencia a grupos.

2. Use Get-ADGroup para obtener el identificador de seguridad (SID) del grupo de seguridad y proporcionarla al administrador de HGS. 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![Comando Get-AdGroup con salida](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrar al SID del grupo de seguridad con HGS  

1. Obtener al SID del grupo de seguridad para hosts protegidos desde el administrador del tejido y ejecute el siguiente comando para registrar el grupo de seguridad con HGS. 
   Vuelva a ejecutar el comando si es necesario para los grupos adicionales. 
   Proporcione un nombre descriptivo para el grupo. 
   No es necesario para que coincida con el nombre del grupo de seguridad de Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Para comprobar que se agregó el grupo, ejecute [Get HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 


