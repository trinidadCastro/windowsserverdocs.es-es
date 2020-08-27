---
title: 'Recuperación de bosque de AD: invalidar el grupo de RID'
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.openlocfilehash: 36919eb4f4e67129446975f9c8c4d60fd64cc499
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939645"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Recuperación de bosque de AD: invalidar el grupo RID actual

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Use el procedimiento siguiente para que Windows PowerShell invalide el grupo de RID actual en un controlador de dominio. Windows PowerShell está habilitado de forma predeterminada en Windows Server 2012 y Windows Server 2008 R2, pero no en Windows Server 2008, donde debe instalarse mediante **Agregar características**. Se puede [Descargar](https://www.microsoft.com/download/details.aspx?id=20020) para ejecutarse en Windows Server 2003.

Para comprobar que el comando se completó correctamente, busque el ID. de evento 16654 (el origen es Directory-Services-SAM) en el registro del sistema en Visor de eventos en Windows Server 2012. Las versiones anteriores de Windows no registran este evento.

> [!NOTE]
> Después de invalidar el grupo de RID, recibirá un error al intentar crear la entidad de seguridad por primera vez (usuario, equipo o grupo). El intento de crear un objeto desencadena una solicitud para un nuevo grupo RID. El reintento de la operación se realiza correctamente porque se asignará el nuevo grupo RID.

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

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
