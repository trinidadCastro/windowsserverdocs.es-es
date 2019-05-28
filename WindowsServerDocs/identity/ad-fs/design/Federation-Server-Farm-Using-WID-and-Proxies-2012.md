---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Granja de servidores de federación con WID y servidores proxy
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 19e73e43a863ec60fbc9da09b24173220bb331ed
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191360"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Granja de servidores de federación con WID y servidores proxy

Esta topología de implementación para los servicios de federación de Active Directory \(AD FS\) es idéntica a la granja de servidores de federación con Windows Internal Database \(WID\) topología, pero agrega servidores proxy de federación a la red perimetral para admitir usuarios externos. Los servidores proxy de federación redirigen las solicitudes de autenticación de cliente que proceden de fuera de la red corporativa a la granja de servidores de federación.  
  
## <a name="deployment-considerations"></a>Consideraciones acerca de la implementación  
Esta sección describen diversas consideraciones acerca de los destinatarios, ventajas y limitaciones asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Las organizaciones con 100 o menos relaciones de confianza configurado que deben proporcionar a sus usuarios internos y externos a los usuarios \(que están conectados a los equipos que se encuentran físicamente fuera de la red corporativa\) con solo inicio de sesión\-en \(SSO\) acceso a aplicaciones federadas o servicios  
  
-   Organizaciones que necesitan para proporcionar a sus usuarios internos y externos a los usuarios con acceso de inicio de sesión único con Microsoft Office 365  
  
-   Organizaciones más pequeñas que tienen los usuarios externos y requieren servicios redundantes, escalables  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Las mismas ventajas, como se muestra para el [granja de servidores de federación con WID](Federation-Server-Farm-Using-WID-2012.md) topología, además de la ventaja de proporcionar acceso adicional para los usuarios externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Las mismas limitaciones que enumeradas para el [granja de servidores de federación con WID](Federation-Server-Farm-Using-WID-2012.md) topología  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de selección de ubicación y la red de servidor  
Para implementar esta topología, además de agregar dos servidores proxy de federación, debe asegurarse de que la red perimetral también puede proporcionar acceso a un sistema de nombres de dominio \(DNS\) server y a un segundo Network Load Balancing \(NLB\) host. El segundo host NLB debe configurarse con un clúster NLB que usa un Internet\-dirección IP del clúster sea accesible y deben usar la misma configuración de nombre DNS de clúster que el clúster NLB anterior que configuró en la red corporativa \( FS.fabrikam.com\). También se deben configurar los servidores proxy de federación con Internet\-direcciones IP accesibles.  
  
La siguiente ilustración muestra la granja de servidores de federación existente con topología WID que se ha descrito anteriormente y cómo la empresa ficticia de Fabrikam, Inc., proporciona acceso a un servidor DNS perimetral, agrega un segundo host NLB con el mismo nombre DNS del clúster \(fs.fabrikam.com\), y agrega dos servidores proxy de federación \(fsp1 y fsp2\) a la red perimetral.  
  
![granja de servidores con WID](media/FarmWIDProxies.gif)  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación o servidores proxy de federación, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md) o [nombre Requisitos de resolución para los servidores proxy de federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
