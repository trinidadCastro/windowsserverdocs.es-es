---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Revisar el rol del servidor de federación en el asociado de cuenta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5bc304277b872bd9b99b79b84694dd0cb1eb73ba
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190882"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Revisar el rol del servidor de federación en el asociado de cuenta

Un servidor de federación de Active Directory Federation Services \(AD FS\) funciona como un emisor de tokens de seguridad. Un servidor de federación genera notificaciones basadas en la cuenta almacenan valores que residen en un atributo local y las empaqueta en tokens de seguridad para que los usuarios puedan acceder a la perfección Web\-explorador\-aplicaciones basadas en \(mediante inicio de sesión único\-en \(SSO\) \) que se hospedan en una organización del asociado de recurso.  
  
> [!NOTE]  
> Cuando los usuarios acceden a aplicaciones federadas mediante un explorador Web, un servidor de federación emite automáticamente las cookies a los usuarios a mantener su estado de inicio de sesión para que Web\-explorador\-aplicación basada en. Estas cookies contienen notificaciones para los usuarios. Las cookies habilitan las capacidades SSO para que los usuarios no tenga que escribirlas cada vez que visitan Web diferente\-explorador\-en función de las aplicaciones en el asociado de recurso.  
  
En el diseño SSO Web, las organizaciones con una red perimetral que desean que los usuarios de Internet para tener acceso a las aplicaciones deben instalar a un servidor proxy de federación en la red perimetral. En el diseño SSO Web federado, debe haber al menos un servidor de federación instalado en la red corporativa de la organización del asociado de cuenta y al menos un servidor de federación instalado en la red corporativa de la organización del asociado de recurso.  
  
> [!NOTE]  
> Antes de configurar un equipo de servidor de federación en la organización del asociado de cuenta, primero debe unir el equipo a cualquier dominio del bosque de Active Directory donde se usará el servidor de federación para autenticar a los usuarios de ese bosque. Para obtener más información, consulte [lista de comprobación: Configuración de un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
