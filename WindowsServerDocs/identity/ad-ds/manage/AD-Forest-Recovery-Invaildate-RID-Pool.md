---
title: "Recuperación de bosque de AD - invalidar el grupo de RID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adfs
ms.openlocfilehash: cb024356ae5f872e93448d73ea54b271fe3fae4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Recuperación de bosque de AD - invalidar el grupo de RID actual  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Usa el siguiente procedimiento para nosotros Windows PowerShell para invalidar el conjunto de RID actual en un controlador de dominio. Windows PowerShell está habilitada de manera predeterminada en Windows Server 2012 y Windows Server 2008 R2, pero no Windows Server 2008 donde debe instalarse mediante **agregar características**. Puede ser [descargado](https://www.microsoft.com/download/details.aspx?id=20020) para ejecutarse en Windows Server 2003.  
  
 Para comprobar el comando se completó correctamente, busca el identificador de evento 16654 (el origen es SAM de servicios de directorio) en el registro del sistema en el Visor de eventos en Windows Server 2012. Las versiones anteriores de Windows no registra este evento.  
  
> [!NOTE]
>  Después de que invalidar el conjunto de RID, recibirás un error cuando primero intenta crear la entidad de seguridad (usuario, grupo o equipo). Intentar crear un objeto desencadena una solicitud de un nuevo grupo RID. Reintentar la operación se realiza correctamente porque se asignará el nuevo grupo RID.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Para invalidar el actual elimina grupo  
  
1.  Abrir una sesión de Windows PowerShell con privilegios elevados, ejecuta el comando siguiente y presiona Intro:  
  
    ```  
    $Domain = New-Object System.DirectoryServices.DirectoryEntry  
    $DomainSid = $Domain.objectSid  
    $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
    $RootDSE.UsePropertyCache = $false  
    $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
    $RootDSE.SetInfo()  
    ```  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
