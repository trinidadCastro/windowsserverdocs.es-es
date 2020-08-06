---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: AD FS granja de servidores de Federación con WID y servidores proxy
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e70359b5a05fed8e7cfb467d3410e12939a1de1b
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87864056"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Granja de servidores de federación con WID y servidores proxy

Esta topología de implementación para Servicios de federación de Active Directory (AD FS) \( AD FS \) es idéntica a la granja de servidores de Federación con la topología WID de Windows Internal Database \( \) , pero agrega servidores proxy de Federación a la red perimetral para admitir usuarios externos. Los proxies de servidor de Federación redirigen las solicitudes de autenticación de cliente que proceden de fuera de la red corporativa a la granja de servidores de Federación.  
  
## <a name="deployment-considerations"></a>Consideraciones de la implementación  
En esta sección se describen varias consideraciones sobre la audiencia, las ventajas y las limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Organizaciones con 100 o menos relaciones de confianza configuradas que necesitan proporcionar a los usuarios internos y a los usuarios externos que han \( iniciado sesión en equipos que están ubicados físicamente fuera de la red corporativa \) con acceso de inicio de sesión único a los \- \( \) servicios o aplicaciones federados.  
  
-   Organizaciones que necesitan proporcionar a los usuarios internos y a los usuarios externos acceso de SSO a Microsoft Office 365  
  
-   Organizaciones más pequeñas que tienen usuarios externos y requieren servicios escalables y redundantes  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Las mismas ventajas que se muestran para la [granja de servidores de Federación con](Federation-Server-Farm-Using-WID-2012.md) la topología WID, además de la ventaja de proporcionar acceso adicional a los usuarios externos.  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Las mismas limitaciones que se muestran para la [granja de servidores de Federación con](Federation-Server-Farm-Using-WID-2012.md) la topología WID  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de ubicación de servidor y diseño de red  
Para implementar esta topología, además de agregar dos servidores proxy de Federación, debe asegurarse de que la red perimetral también puede proporcionar acceso a un servidor DNS del sistema de nombres de dominio \( \) y a un segundo host NLB de equilibrio de carga de red \( \) . El segundo host de NLB debe configurarse con un clúster de NLB que use una \- dirección IP de clúster accesible por Internet y debe usar la misma configuración de nombre DNS de clúster que el clúster NLB anterior configurado en la red corporativa \( FS.fabrikam.com \) . Los servidores proxy de Federación también deben configurarse con \- direcciones IP accesibles por Internet.  
  
La siguiente ilustración muestra la granja de servidores de Federación existente con la topología WID que se ha descrito anteriormente y cómo la empresa ficticia Fabrikam, Inc., proporciona acceso a un servidor DNS perimetral, agrega un segundo host de NLB con el mismo nombre DNS de clúster \( FS.fabrikam.com \) y agrega dos servidores proxy \( de Federación fsp1 y fsp2 \) a la red perimetral.  
  
![granja de servidores con WID](media/FarmWIDProxies.gif)  
  
Para obtener más información acerca de cómo configurar el entorno de red para su uso con servidores de Federación o proxies de servidor de Federación, consulte [requisitos de resolución de nombres para servidores de Federación](Name-Resolution-Requirements-for-Federation-Servers.md) o [requisitos de resolución de nombres para los proxies de servidor de Federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
