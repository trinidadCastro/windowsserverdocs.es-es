---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: "Revisa el rol de servidor de federación de asociado de recurso"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Revisa el rol de servidor de federación de asociado de recurso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El servidor de federación en el recurso partner organización intercepta entrante tokens de seguridad que se envían mediante un servidor de federación de cuenta, valida y firma y, a continuación, emite sus propio tokens de seguridad que están destinados a la aplicación basada en Web\.  
  
> [!NOTE]  
> Cuando los usuarios federados usan sus exploradores Web para acceder a las aplicaciones basadas en Web\, el servidor de federación de la organización de partner de recursos crea una nueva cookie de autenticación y escribe en el explorador. Esta cookie permite single\ sign\ en \(SSO\) funcionalidades para que los usuarios no tienen vuelva a iniciar sesión en el servidor de federación de asociado de la cuenta cuando los usuarios intentan obtener acceso a diferentes Web\-aplicaciones del asociado de recurso.  
  
En el diseño Web SSO, el servidor de federación de al menos un debe estar instalado en la red perimetral. En el diseño SSO Web federado, debe haber al menos un servidor de federación instalado en la red corporativa de la organización de partner de la cuenta y al menos un servidor de federación instalado en la red corporativa de la organización de partner de recursos.  
  
> [!NOTE]  
> Antes de configurar un equipo de servidor de federación de la organización de partner de recurso, primero debes unir el equipo a cualquier dominio de Active Directory en la organización de partner de recurso. Para obtener más información, consulta [lista de comprobación: configurar un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

