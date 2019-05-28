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
ms.openlocfilehash: b2ed7a09bbc50c83d3bf6f8f2688152ed5202abc
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190821"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Revisar el rol del servidor de federación en el asociado de recurso

El servidor de federación en la organización del asociado de recurso intercepta los tokens de seguridad entrantes que se envían por un servidor de federación de cuenta, valida y firma de ellos y, a continuación, emite su propio tokens de seguridad que están destinadas a la Web\-basado aplicación.  
  
> [!NOTE]  
> Cuando los usuarios federados usan sus exploradores Web para tener acceso a Web\-aplicaciones basadas en, el servidor de federación en la organización del asociado de recurso crea una nueva cookie de autenticación y lo escribe en el explorador. Esta cookie permite solo\-sesión\-en \(SSO\) capacidades para que no tengan los usuarios vuelvan a iniciar sesión en el servidor de federación del asociado de cuenta cuando los usuarios intentan tener acceso a Web diferentes\- aplicaciones basadas en el asociado de recurso.  
  
En el diseño SSO Web, al menos un servidor de federación debe instalarse en la red perimetral. En el diseño SSO Web federado, debe haber al menos un servidor de federación instalado en la red corporativa de la organización del asociado de cuenta y al menos un servidor de federación instalado en la red corporativa de la organización del asociado de recurso.  
  
> [!NOTE]  
> Antes de configurar un equipo de servidor de federación en la organización del asociado de recurso, primero debe unir el equipo a cualquier dominio de Active Directory en la organización del asociado de recurso. Para obtener más información, consulte [lista de comprobación: Configuración de un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

