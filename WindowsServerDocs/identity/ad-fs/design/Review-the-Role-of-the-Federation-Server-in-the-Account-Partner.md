---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Revisar el rol del servidor de federación en el asociado de cuenta
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2d51f0091746d2d5e816108e3b92d0cb1a8267cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858538"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Revisar el rol del servidor de federación en el asociado de cuenta

Un servidor de Federación en Servicios de federación de Active Directory (AD FS) \(AD FS\) funciona como un emisor de tokens de seguridad. Un servidor de Federación genera notificaciones basadas en valores de cuenta que residen en un almacén de atributos local y las empaqueta en tokens de seguridad para que los usuarios puedan acceder sin problemas a aplicaciones basadas en el explorador de\-web\-\(mediante el inicio de\-sesión único en \(SSO\)\) que se hospedan en una organización del asociado de recurso.  
  
> [!NOTE]  
> Cuando los usuarios acceden a aplicaciones federadas mediante un explorador Web, un servidor de Federación emite automáticamente las cookies a los usuarios para mantener el estado de inicio de sesión de la aplicación basada en\-del explorador de\-Web. Estas cookies contienen notificaciones para los usuarios. Las cookies habilitan las capacidades de SSO para que los usuarios no tengan que escribir credenciales cada vez que visiten diferentes aplicaciones basadas en\-explorador de\-Web en el asociado de recurso.  
  
En el diseño SSO Web, las organizaciones con una red perimetral que quiera que los usuarios de Internet tengan acceso a las aplicaciones deben instalar un servidor proxy de Federación en la red perimetral. En el diseño de SSO Web federado, debe haber al menos un servidor de Federación instalado en la red corporativa de la organización del asociado de cuenta y al menos un servidor de Federación instalado en la red corporativa de la organización del asociado de recurso.  
  
> [!NOTE]  
> Para poder configurar un equipo de servidor de Federación en la organización del asociado de cuenta, primero debe unir el equipo a cualquier dominio del bosque de Active Directory donde se utilizará el servidor de Federación para autenticar a los usuarios de ese bosque. Para obtener más información, consulte [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
