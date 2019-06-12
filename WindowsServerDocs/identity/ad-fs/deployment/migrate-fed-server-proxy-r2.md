---
title: Migrar el servidor de proxy de AD FS 2.0 federation
description: Proporciona información sobre cómo migrar un servidor proxy AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a2eb6c670e704564bed49486b8950dab96da8a80
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444572"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrar el servidor de Proxy de servicios de federación de Active Directory a Windows Server 2012 R2

En Active Directory Federation Services (AD FS) en Windows Server 2012 R2, el rol de un servidor proxy de federación se gestiona mediante un nuevo servicio de rol acceso remoto denominado a Web Application Proxy. En Windows Server 2012 R2, para habilitar AD FS para el acceso desde fuera de la red corporativa, puede implementar a uno o más Proxies de aplicación Web. Sin embargo, no puede migrar a un servidor proxy de federación que se ejecutan en Windows Server 2008 R2 o Windows Server 2012 a un Proxy de aplicación Web que se ejecutan en Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  NO se admite la migración de un servidor proxy de federación que se ejecutan en Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a un Proxy de aplicación Web que se ejecutan en Windows Server 2012 R2.  
  
Si desea configurar AD FS en una granja de Windows Server 2012 R2 migrada para el acceso de extranet, debe realizar una nueva implementación de uno o más equipos Web Application Proxy como parte de la infraestructura de AD FS.  
  
Para planear la implementación de Web Application Proxy, puede consultar la información de los temas siguientes:  
  
- [Planear la infraestructura del Proxy de aplicación Web](https://technet.microsoft.com/library/dn383648.aspx)  
  
- [Planear el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
  Para implementar Web Application Proxy, puede seguir los procedimientos de los temas siguientes:  
  
- [Configurar la infraestructura del Proxy de aplicación Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
- [Instalar y configurar el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar servicios de rol de servicios de federación de Active Directory a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparar la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)

