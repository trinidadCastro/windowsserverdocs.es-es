---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Implementación de servidores proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 330214e83b6da5bf711c36995306f8f1a098fa24
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192213"
---
# <a name="deploying-federation-server-proxies"></a>Implementación de servidores proxy de federación

En Active Directory Federation Services \(AD FS\) en Windows Server 2012 R2, el rol de un servidor proxy de federación se gestiona mediante un nuevo servicio de rol acceso remoto denominado Web Application Proxy. Para habilitar AD FS para el acceso desde fuera de la red corporativa, lo que era el propósito de la implementación de un servidor proxy de federación en versiones heredadas de AD FS, como AD FS 2.0 y AD FS en Windows Server 2012, puede implementar a uno o varios web application proxy para un D FS en Windows Server 2012 R2.  
  
En el contexto de AD FS, Proxy de aplicación Web funciona como un servidor proxy de federación de AD FS. Además, el servicio Proxy de aplicación web ofrece funcionalidad de proxy inverso para las aplicaciones web dentro de la red corporativa para permitir a los usuarios de cualquier dispositivo acceso a estas desde fuera de la red corporativa. Para obtener información general sobre el servicio de rol Proxy de aplicación web, consulte la introducción a Proxy de aplicación web.  
  
Para planear la implementación de Proxy de aplicación web, puede consultar la información de los temas siguientes:  
  
-   [Planear la infraestructura del Proxy de aplicación Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Planear el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Para implementar Web Application Proxy, puede seguir los procedimientos de los temas siguientes:  
  
-   [Configurar la infraestructura del Proxy de aplicación Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Instalar y configurar el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Vea también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

