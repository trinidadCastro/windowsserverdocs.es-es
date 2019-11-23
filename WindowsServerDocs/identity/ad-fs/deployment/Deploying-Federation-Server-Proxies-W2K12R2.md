---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Implementación de servidores proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 34d2a15d5ad4f2563beffbce6ae5e729cf72c3ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359677"
---
# <a name="deploying-federation-server-proxies"></a>Implementación de servidores proxy de federación

En Servicios de federación de Active Directory (AD FS) \(AD FS\) en Windows Server 2012 R2, el rol de un servidor proxy de Federación se administra mediante un nuevo servicio de rol de acceso remoto denominado proxy de aplicación Web. Para habilitar la AD FS para la accesibilidad desde fuera de la red corporativa, que era el propósito de implementar un servidor proxy de Federación en versiones heredadas de AD FS, como AD FS 2,0 y AD FS en Windows Server 2012, puede implementar uno o varios proxy de aplicación web para un D FS en Windows Server 2012 R2.  
  
En el contexto de AD FS, el proxy de aplicación web funciona como un proxy de servidor de Federación AD FS. Además, el servicio Proxy de aplicación web ofrece funcionalidad de proxy inverso para las aplicaciones web dentro de la red corporativa para permitir a los usuarios de cualquier dispositivo acceso a estas desde fuera de la red corporativa. Para obtener información general sobre el servicio de rol Proxy de aplicación web, consulte la introducción a Proxy de aplicación web.  
  
Para planear la implementación de Proxy de aplicación web, puede consultar la información de los temas siguientes:  
  
-   [Planear la infraestructura del proxy de aplicación web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Planear el servidor proxy de aplicación Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Para implementar Web Application Proxy, puede seguir los procedimientos de los temas siguientes:  
  
-   [Configurar la infraestructura del proxy de aplicación Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Instalación y configuración del servidor proxy de aplicación Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Consulta también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de AD FS de Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

