---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: "Federación granja de servidores con WID y servidores proxy"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 92d3da7cf7ab8596d6e2866543701bf530753dae
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Federación granja de servidores con WID y servidores proxy

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Esta topología de implementación para los servicios de federación de Active Directory \(AD FS\) es idéntica a la granja de servidores de federación con topología \(WID\) base de datos interna de Windows, pero agrega equipos de proxy a la red perimetral para admitir los usuarios externos. Estos servidores proxy redirigen las solicitudes de autenticación de cliente que provienen de fuera de la red corporativa a los servidores de federación. En versiones anteriores de AD FS, estos servidores proxy llamó a proxies de servidor de federación.  
  
> [!IMPORTANT]  
> En los servicios de federación de Active Directory \(AD FS\) en Windows Server 2012 R2, el rol de un proxy de servidor de federación está administrado por un nuevo servicio de rol de acceso remoto denominado a Proxy de aplicación Web. Para habilitar la AD FS de accesibilidad desde fuera de la red corporativa, que era el propósito de la implementación de un proxy de servidor de federación en versiones heredadas de AD FS, por ejemplo, AD FS 2.0 y AD FS en Windows Server 2012, puedes implementar a uno o más proxy de aplicación web de AD FS en Windows Server 2012 R2.  
>   
> En el contexto de AD FS, Proxy de aplicación Web funciona como un proxy de servidor de federación de AD FS. Además, el Proxy de aplicación Web proporciona funcionalidad de proxy inverso para aplicaciones web dentro de la red corporativa permitir que los usuarios en cualquier dispositivo acceder a ellos desde fuera de la red corporativa. Para obtener más información sobre el servicio de rol Proxy de aplicación Web, consulta la información general sobre el Proxy de aplicación Web.  
>   
> Para planear la implementación de proxy de aplicación Web, puedes revisar la información en los siguientes temas:  
>   
> -   [Planear la infraestructura de Proxy de aplicación Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Planear el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Consideraciones de implementación  
Esta sección describe diversas consideraciones acerca de la audiencia de destino, ventajas y limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Las organizaciones con 100 configurado las relaciones de confianza que se deben proporcionar tanto sus usuarios internos y externos a los usuarios \ (que se iniciaron sesión en equipos que se encuentran físicamente fuera el red\ corporativa) con solo sign\ \(SSO\) acceso a los servicios o aplicaciones federadas  
  
-   Organizaciones que necesitan proporcionar tanto sus usuarios internos y externos a los usuarios con acceso de inicio de sesión ÚNICO a Microsoft Office 365  
  
-   Organizaciones pequeñas que tienen los usuarios externos y requieren servicios redundantes, escalables  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Las mismas ventajas, como se muestra para la [federación servidor granja usando WID](Federation-Server-Farm-Using-WID.md) topología, además de la ventaja de proporcionar acceso adicional para los usuarios externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Las mismas limitaciones según se indiquen para la [federación servidor granja usando WID](Federation-Server-Farm-Using-WID.md) topología  

||1 \-100 confianzas de punto de reunión|Más de 100 confianzas de punto de Reunión 
| ----- |-----| ------ |
|1 \-30 AD FS nodos|WID compatible|No se admite con WID \-SQL necesario 
|Más de 30 AD FS nodos|No se admite con WID \-SQL necesario|No se admite con WID \-SQL necesario  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de red y la ubicación de servidor  
Para implementar esta topología, además de agregar dos proxy de aplicación web, debe asegurarse de que la red perimetral también puede proporcionar acceso a un servidor de sistema de nombres de dominio \(DNS\) y a un segundo host \(NLB\) equilibrio de carga de red. El segundo host NLB debe estar configurado con un clúster NLB que use una dirección IP de clúster Internet\ accesible y debe usar la misma configuración de nombre de clúster DNS como el clúster NLB anterior que configuraste en el \(fs.fabrikam.com\) red corporativa. El proxy de aplicación web también deben estar configurados con direcciones IP Internet\ accesible.  
  
La siguiente ilustración muestra la granja de servidores de federación existente con topología WID que se ha descrito anteriormente, y cómo la empresa ficticia de Fabrikam, Inc., proporciona acceso a un servidor DNS perímetro, agrega un segundo host NLB con la misma clúster DNS nombre \(fs.fabrikam.com\) y agrega dos proxy de aplicación web \(wap1 and wap2\) a la red de perímetro.  
  
![Proxies y WID granja](media/WIDFarmADFSBlue.gif)  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación o proxy de aplicación web, consulta "Requisitos de resolución de nombre de" sección [AD FS requisitos](AD-FS-Requirements.md) y [planear la infraestructura de Proxy de aplicación Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Consulta también  
[Planear la topología de implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

