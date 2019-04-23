---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Granja de servidores de federación con WID y servidores proxy
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e372f066fc82b9857d438234b491732a177e24fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860396"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Granja de servidores de federación con WID y servidores proxy

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Esta topología de implementación para los servicios de federación de Active Directory \(AD FS\) es idéntica a la granja de servidores de federación con Windows Internal Database \(WID\) topología, pero agrega los equipos de proxy para el red perimetral para admitir usuarios externos. Estos servidores proxy redirigen las solicitudes de autenticación de cliente que proceden de fuera de la red corporativa a la granja de servidores de federación. En versiones anteriores de AD FS, estos servidores proxy se denominaban a servidores proxy de federación.  
  
> [!IMPORTANT]  
> En Active Directory Federation Services \(AD FS\) en Windows Server 2012 R2, el rol de un servidor proxy de federación se gestiona mediante un nuevo servicio de rol acceso remoto denominado Web Application Proxy. Para habilitar AD FS para el acceso desde fuera de la red corporativa, lo que era el propósito de la implementación de un servidor proxy de federación en versiones heredadas de AD FS, como AD FS 2.0 y AD FS en Windows Server 2012, puede implementar a uno o varios web application proxy para un D FS en Windows Server 2012 R2.  
>   
> En el contexto de AD FS, Proxy de aplicación Web funciona como un servidor proxy de federación de AD FS. Además, el servicio Proxy de aplicación web ofrece funcionalidad de proxy inverso para las aplicaciones web dentro de la red corporativa para permitir a los usuarios de cualquier dispositivo acceso a estas desde fuera de la red corporativa. Para obtener información general sobre el servicio de rol Proxy de aplicación web, consulte la introducción a Proxy de aplicación web.  
>   
> Para planear la implementación de Proxy de aplicación web, puede consultar la información de los temas siguientes:  
>   
> -   [Planear la infraestructura del Proxy de aplicación Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Planear el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Consideraciones acerca de la implementación  
Esta sección describen diversas consideraciones acerca de los destinatarios, ventajas y limitaciones asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Las organizaciones con 100 o menos relaciones de confianza configurado que deben proporcionar a sus usuarios internos y externos a los usuarios \(que están conectados a los equipos que se encuentran físicamente fuera de la red corporativa\) con solo inicio de sesión\-en \(SSO\) acceso a aplicaciones federadas o servicios  
  
-   Organizaciones que necesitan para proporcionar a sus usuarios internos y externos a los usuarios con acceso de inicio de sesión único con Microsoft Office 365  
  
-   Organizaciones más pequeñas que tienen los usuarios externos y requieren servicios redundantes, escalables  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Las mismas ventajas, como se muestra para el [granja de servidores de federación con WID](Federation-Server-Farm-Using-WID.md) topología, además de la ventaja de proporcionar acceso adicional para los usuarios externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Las mismas limitaciones que enumeradas para el [granja de servidores de federación con WID](Federation-Server-Farm-Using-WID.md) topología  

||1 \- 100 de RP de confianzas|Más de 100 de RP de confianzas 
| ----- |-----| ------ |
|1 \- 30 AD FS nodos|WID admitida|No se admite con WID \- SQL necesario 
|Nodos de más de 30 AD FS|No se admite con WID \- SQL necesario|No se admite con WID \- SQL necesario  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de selección de ubicación y la red de servidor  
Para implementar esta topología, además de agregar dos web proxy de aplicación, debe asegurarse de que la red perimetral también puede proporcionar acceso a un sistema de nombres de dominio \(DNS\) server y a un segundo Network Load Balancing \( NLB\) host. El segundo host NLB debe configurarse con un clúster NLB que usa un Internet\-dirección IP del clúster sea accesible y deben usar la misma configuración de nombre DNS de clúster que el clúster NLB anterior que configuró en la red corporativa \( FS.fabrikam.com\). También se deben configurar el proxy de aplicación web con Internet\-direcciones IP accesibles.  
  
La siguiente ilustración muestra la granja de servidores de federación existente con topología WID que se ha descrito anteriormente y cómo la empresa ficticia de Fabrikam, Inc., proporciona acceso a un servidor DNS perimetral, agrega un segundo host NLB con el mismo nombre DNS del clúster \(fs.fabrikam.com\)y agrega dos proxies de aplicación web \(fsp1 y fsp2\) a la red perimetral.  
  
![Servidores proxy y la granja WID](media/WIDFarmADFSBlue.gif)  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación o servidores proxy de aplicación web, vea "Requisitos de resolución de nombre" sección [requisitos de AD FS](AD-FS-Requirements.md) y [Plan Web Infraestructura del Proxy de aplicación (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Vea también  
[Planear la topología de implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  
