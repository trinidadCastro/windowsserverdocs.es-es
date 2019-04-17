---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: "Implementar el proxy de servidor de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dc49d8f4b656fdbb92083aa3c60bc4ce81091e9b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-server-proxies"></a>Implementar el proxy de servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En los servicios de federación de Active Directory \(AD FS\) en Windows Server 2012 R2, el rol de un proxy de servidor de federación está administrado por un nuevo servicio de rol de acceso remoto denominado a Proxy de aplicación Web. Para habilitar la AD FS de accesibilidad desde fuera de la red corporativa, que era el propósito de la implementación de un proxy de servidor de federación en versiones heredadas de AD FS, por ejemplo, AD FS 2.0 y AD FS en Windows Server 2012, puedes implementar a uno o más proxy de aplicación web de AD FS en Windows Server 2012 R2.  
  
En el contexto de AD FS, Proxy de aplicación Web funciona como un proxy de servidor de federación de AD FS. Además, el Proxy de aplicación Web proporciona funcionalidad de proxy inverso para aplicaciones web dentro de la red corporativa permitir que los usuarios en cualquier dispositivo acceder a ellos desde fuera de la red corporativa. Para obtener más información sobre el servicio de rol Proxy de aplicación Web, consulta la información general sobre el Proxy de aplicación Web.  
  
Para planear la implementación de proxy de aplicación Web, puedes revisar la información en los siguientes temas:  
  
-   [Planear la infraestructura de Proxy de aplicación Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Planear el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Para implementar el proxy de aplicación Web, puedes seguir los procedimientos descritos en los siguientes temas:  
  
-   [Configurar la infraestructura de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Instalar y configurar el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Consulta también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar un conjunto de servidor de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

