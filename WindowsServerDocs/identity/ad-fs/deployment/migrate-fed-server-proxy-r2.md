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
ms.openlocfilehash: 57367cadd3c7ce3d031c6eb3a53c333422543dae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359373"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrar el servidor proxy Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2

En Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2, el rol de un servidor proxy de Federación se administra mediante un nuevo servicio de rol de acceso remoto denominado proxy de aplicación Web. En Windows Server 2012 R2, para habilitar la AD FS para la accesibilidad desde fuera de la red corporativa, puede implementar uno o más proxies de aplicación Web. Sin embargo, no puede migrar un servidor proxy de Federación que se ejecute en Windows Server 2008 R2 o Windows Server 2012 a un proxy de aplicación web que se ejecute en Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  NO se admite la migración de un servidor proxy de Federación que se ejecute en Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a un proxy de aplicación web que se ejecute en Windows Server 2012 R2.  
  
Si desea configurar AD FS en una granja de servidores de Windows Server 2012 R2 migrada para el acceso a Extranet, debe realizar una nueva implementación de uno o más equipos de proxy de aplicación web como parte de la infraestructura de AD FS.  
  
Para planear la implementación de Web Application Proxy, puede consultar la información de los temas siguientes:  
  
- [Planear la infraestructura del proxy de aplicación Web](https://technet.microsoft.com/library/dn383648.aspx)  
  
- [Planear el servidor proxy de aplicación Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
  Para implementar Web Application Proxy, puede seguir los procedimientos de los temas siguientes:  
  
- [Configurar la infraestructura del proxy de aplicación Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
- [Instalación y configuración del servidor proxy de aplicación Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparación de la migración del servidor de Federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migración del servidor de Federación de AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)

