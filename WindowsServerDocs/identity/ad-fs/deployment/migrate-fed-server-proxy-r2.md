---
title: Migrar el servidor proxy de Federación AD FS 2,0
description: Proporciona información sobre cómo migrar un servidor proxy AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9a9b5f6454ec93ec27f557588016295935827c18
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966217"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrar el servidor proxy Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2

En Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2, el rol de un servidor proxy de Federación se administra mediante un nuevo servicio de rol de acceso remoto denominado proxy de aplicación Web. En Windows Server 2012 R2, para habilitar la AD FS para la accesibilidad desde fuera de la red corporativa, puede implementar uno o más proxies de aplicación Web. Sin embargo, no puede migrar un servidor proxy de Federación que se ejecute en Windows Server 2008 R2 o Windows Server 2012 a un proxy de aplicación web que se ejecute en Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  NO se admite la migración de un servidor proxy de Federación que se ejecute en Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a un proxy de aplicación web que se ejecute en Windows Server 2012 R2.  
  
Si desea configurar AD FS en una granja de servidores de Windows Server 2012 R2 migrada para el acceso a Extranet, debe realizar una nueva implementación de uno o más equipos de proxy de aplicación web como parte de la infraestructura de AD FS.  
  
Para planear la implementación de Web Application Proxy, puede consultar la información de los temas siguientes:  
  
- [Planear la infraestructura del Proxy de aplicación web](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))  
  
- [Planear el servidor de Proxy de aplicación web](/previous-versions/orphan-topics/ws.11/dn383647(v=ws.11))  
  
  Para implementar Web Application Proxy, puede seguir los procedimientos de los temas siguientes:  
  
- [Configurar la infraestructura del Proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383644(v=ws.11))  
  
- [Instalar y configurar el servidor de Proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383662(v=ws.11))  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparación de la migración del servidor de Federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migración del servidor de Federación de AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Comprobación de la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)
