---
title: 'Recuperación de bosques de AD: invalidar el grupo de RID'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 46115991e48da301a8a739009bac27415ebe73df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842516"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Recuperación de bosques de AD: invalidar el grupo RID actual  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Utilice el procedimiento siguiente para nosotros, Windows PowerShell para invalidar el grupo RID actual en un controlador de dominio. Windows PowerShell está habilitada de forma predeterminada en Windows Server 2012 y Windows Server 2008 R2, pero no Windows Server 2008 donde debe instalarse mediante **agregar características**. Puede ser [descargado](https://www.microsoft.com/download/details.aspx?id=20020) para ejecutarse en Windows Server 2003.  

Para comprobar que el comando se completó correctamente, busque el Id. de evento 16654 (el origen es Directory-Services-SAM) en el registro del sistema en el Visor de eventos de Windows Server 2012. Las versiones anteriores de Windows no registra este evento.  
  
> [!NOTE]
> Después de invalidar el grupo RID, recibirá un error al intentar crear la entidad de seguridad (usuario, equipo o grupo) en primer lugar. Error al intentar crear un objeto desencadena una solicitud de un nuevo grupo RID. Reintento de la operación se realiza correctamente porque el nuevo grupo RID que se va a asignar.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Para invalidar el grupo RID actual  
  
- Abra una sesión de Windows PowerShell con privilegios elevados, ejecute el siguiente comando y presione ENTRAR:  

   ```powershell
   $Domain = New-Object System.DirectoryServices.DirectoryEntry  
   $DomainSid = $Domain.objectSid  
   $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
   $RootDSE.UsePropertyCache = $false  
   $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
   $RootDSE.SetInfo()  
   ```  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
