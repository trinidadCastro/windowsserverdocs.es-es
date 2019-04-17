---
title: "Migrar el servidor proxy de federación 2.0 de AD FS"
description: "Proporciona información sobre cómo migrar un servidor proxy de AD FS a Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 33ab29fd5efdb0bdd1fe25580e3f4434071e1c7d
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2017
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrar el servidor de Proxy de servicios de federación de Active Directory para Windows Server 2012 R2

Servicios de Active Directory federación (AD FS) en Windows Server 2012 R2, el rol de un proxy de servidor de federación está administrado por un nuevo servicio de rol de acceso remoto denominado a Proxy de aplicación Web. En Windows Server 2012 R2, para habilitar la AD FS de accesibilidad desde fuera de la red corporativa, puedes implementar a uno o más proxy de aplicación Web. Sin embargo, no puede migrar a un proxy de servidor de federación ejecutando en Windows Server 2008 R2 o Windows Server 2012a un Proxy de aplicación Web ejecutándose en Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  NO se admite la migración de un proxy de servidor de federación ejecutando en Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012a un Proxy de aplicación Web ejecutándose en Windows Server 2012 R2.  
  
Si quieres configurar AD FS en un conjunto de Windows Server 2012 R2 migrado obtener acceso a la extranet, debes realizar una implementación fresca de uno o más equipos de Proxy de aplicación Web como parte de la infraestructura de AD FS.  
  
Para planear la implementación de Proxy de aplicación Web, puedes revisar la información en los siguientes temas:  
  
-   [Planear la infraestructura de Proxy de aplicación Web](https://technet.microsoft.com/en-us/library/dn383648.aspx)  
  
-   [Planear el servidor de Proxy de aplicación Web](https://technet.microsoft.com/en-us/library/dn383647.aspx)  
  
 Para implementar el proxy de aplicación Web, puedes seguir los procedimientos descritos en los siguientes temas:  
  
-   [Configurar la infraestructura de Proxy de aplicación Web](https://technet.microsoft.com/en-us/library/dn383644.aspx)  
  
-   [Instalar y configurar el servidor de Proxy de aplicación Web](https://technet.microsoft.com/en-us/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar los servicios de rol de servicios de federación de Active Directory para Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparación para migrar el servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)

