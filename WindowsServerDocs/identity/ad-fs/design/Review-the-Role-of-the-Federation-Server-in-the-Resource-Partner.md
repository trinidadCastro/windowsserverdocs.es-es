---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Revisar el rol del servidor de federación en el asociado de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841846"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Revisar el rol del servidor de federación en el asociado de recurso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El servidor de federación en la organización del asociado de recurso intercepta los tokens de seguridad entrantes que se envían por un servidor de federación de cuenta, valida y firma de ellos y, a continuación, emite su propio tokens de seguridad que están destinadas a la Web\-basado aplicación.  
  
> [!NOTE]  
> Cuando los usuarios federados usan sus exploradores Web para tener acceso a Web\-aplicaciones basadas en, el servidor de federación en la organización del asociado de recurso crea una nueva cookie de autenticación y lo escribe en el explorador. Esta cookie permite solo\-sesión\-en \(SSO\) capacidades para que no tengan los usuarios vuelvan a iniciar sesión en el servidor de federación del asociado de cuenta cuando los usuarios intentan tener acceso a Web diferentes\- aplicaciones basadas en el asociado de recurso.  
  
En el diseño SSO Web, al menos un servidor de federación debe instalarse en la red perimetral. En el diseño SSO Web federado, debe haber al menos un servidor de federación instalado en la red corporativa de la organización del asociado de cuenta y al menos un servidor de federación instalado en la red corporativa de la organización del asociado de recurso.  
  
> [!NOTE]  
> Antes de configurar un equipo de servidor de federación en la organización del asociado de recurso, primero debe unir el equipo a cualquier dominio de Active Directory en la organización del asociado de recurso. Para obtener más información, consulte [lista de comprobación: Configuración de un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

