---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Granja de servidores de federación con WID y servidores proxy
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 60072037aea4ecd81376e1334f3a89b7bb2ff851
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408087"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Granja de servidores de federación con WID y servidores proxy

Esta topología de implementación de Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 es idéntica a la de la granja de servidores de Federación con Windows Internal Database \(WID @ no__t-3, pero agrega proxies de servidor de Federación a la red perimetral a admitir usuarios externos. Los proxies de servidor de Federación redirigen las solicitudes de autenticación de cliente que proceden de fuera de la red corporativa a la granja de servidores de Federación.  
  
## <a name="deployment-considerations"></a>Consideraciones acerca de la implementación  
En esta sección se describen varias consideraciones sobre la audiencia, las ventajas y las limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Las organizaciones con 100 o menos relaciones de confianza configuradas que necesiten proporcionar a los usuarios internos y a los usuarios externos \(who inician sesión en equipos que están ubicados físicamente fuera de la red corporativa @ no__t-1 con el signo único @ no__t-2on \(SSO @ no__t-4 acceso a servicios o aplicaciones federados  
  
-   Organizaciones que necesitan proporcionar a los usuarios internos y a los usuarios externos acceso de SSO a Microsoft Office 365  
  
-   Organizaciones más pequeñas que tienen usuarios externos y requieren servicios escalables y redundantes  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Las mismas ventajas que se muestran para la [granja de servidores de Federación con](Federation-Server-Farm-Using-WID-2012.md) la topología WID, además de la ventaja de proporcionar acceso adicional a los usuarios externos.  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Las mismas limitaciones que se muestran para la [granja de servidores de Federación con](Federation-Server-Farm-Using-WID-2012.md) la topología WID  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de ubicación de servidor y diseño de red  
Para implementar esta topología, además de agregar dos servidores proxy de Federación, debe asegurarse de que la red perimetral también puede proporcionar acceso a un sistema de nombres de dominio \(DNS @ no__t-1 y a un segundo host de equilibrio de carga de red \(NLB @ no__t-3. El segundo host de NLB debe configurarse con un clúster de NLB que use una dirección IP de clúster de Internet @ no__t-0accessible y debe usar la misma configuración de nombre DNS de clúster que el clúster NLB anterior configurado en la red corporativa @no__t -1fs. fabrikam. com @ no__t-2. Los servidores proxy de Federación también deben configurarse con direcciones IP de Internet @ no__t-0accessible.  
  
En la siguiente ilustración se muestra la granja de servidores de Federación existente con la topología WID que se ha descrito anteriormente y cómo es el Inc., la compañía proporciona acceso a un servidor DNS perimetral, agrega un segundo host de NLB con el mismo nombre DNS de clúster @no__t -0fs. fabrikam. com @ no__t-1 y agrega dos proxies de servidor de Federación \(fsp1 y fsp2 @ no__t-3 a la red perimetral.  
  
![granja de servidores con WID](media/FarmWIDProxies.gif)  
  
Para obtener más información acerca de cómo configurar el entorno de red para su uso con servidores de Federación o proxies de servidor de Federación, consulte [requisitos de resolución de nombres para servidores de Federación](Name-Resolution-Requirements-for-Federation-Servers.md) o [requisitos de resolución de nombres para la Federación Servidores proxy](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
