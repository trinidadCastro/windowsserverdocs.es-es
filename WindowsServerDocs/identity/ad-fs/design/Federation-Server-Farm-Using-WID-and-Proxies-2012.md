---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Granja de servidores de federación con WID y servidores proxy
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9c6dba880b80de43bb713d1b4495f0e03d56a695
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853098"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Granja de servidores de federación con WID y servidores proxy

Esta topología de implementación de Servicios de federación de Active Directory (AD FS) \(AD FS\) es idéntica a la granja de servidores de Federación con la topología de Windows Internal Database \(WID\), pero agrega proxies de servidor de Federación a la red perimetral para admitir usuarios externos. Los proxies de servidor de Federación redirigen las solicitudes de autenticación de cliente que proceden de fuera de la red corporativa a la granja de servidores de Federación.  
  
## <a name="deployment-considerations"></a>Consideraciones acerca de la implementación  
En esta sección se describen varias consideraciones sobre la audiencia, las ventajas y las limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Organizaciones con 100 o menos relaciones de confianza configuradas que necesitan proporcionar a los usuarios internos y a los usuarios externos \(que han iniciado sesión en equipos ubicados físicamente fuera de la red corporativa\) con el inicio de sesión único\-en \(SSO\) acceso a servicios o aplicaciones federados  
  
-   Organizaciones que necesitan proporcionar a los usuarios internos y a los usuarios externos acceso de SSO a Microsoft Office 365  
  
-   Organizaciones más pequeñas que tienen usuarios externos y requieren servicios escalables y redundantes  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Las mismas ventajas que se muestran para la [granja de servidores de Federación con](Federation-Server-Farm-Using-WID-2012.md) la topología WID, además de la ventaja de proporcionar acceso adicional a los usuarios externos.  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Las mismas limitaciones que se muestran para la [granja de servidores de Federación con](Federation-Server-Farm-Using-WID-2012.md) la topología WID  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de ubicación de servidor y diseño de red  
Para implementar esta topología, además de agregar dos servidores proxy de Federación, debe asegurarse de que la red perimetral también puede proporcionar acceso a un sistema de nombres de dominio \(servidor DNS\) y a un segundo equilibrio de carga de red \(NLB\) host. El segundo host de NLB debe configurarse con un clúster de NLB que use una dirección IP de clúster accesible de Internet\-y debe usar la misma configuración de nombre DNS de clúster que el clúster NLB anterior configurado en la red corporativa \(fs.fabrikam.com\). Los servidores proxy de Federación también deben configurarse con direcciones IP accesibles a través de Internet\-.  
  
En la ilustración siguiente se muestra la granja de servidores de Federación existente con la topología WID que se ha descrito anteriormente y cómo la empresa ficticia Fabrikam, Inc., proporciona acceso a un servidor DNS perimetral, agrega un segundo host de NLB con el mismo nombre DNS de clúster \(fs.fabrikam.com\)y agrega dos servidores proxy de Federación \(fsp1 y fsp2\) a la red perimetral.  
  
![granja de servidores con WID](media/FarmWIDProxies.gif)  
  
Para obtener más información acerca de cómo configurar el entorno de red para su uso con servidores de Federación o proxies de servidor de Federación, consulte [requisitos de resolución de nombres para servidores de Federación](Name-Resolution-Requirements-for-Federation-Servers.md) o [requisitos de resolución de nombres para los proxies de servidor de Federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
