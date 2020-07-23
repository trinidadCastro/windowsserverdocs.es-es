---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guía de diseño de AD FS en Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f0867c3dff104cbaf68a9d6850ceb1af20da62cd
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965507"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guía de diseño de AD FS en Windows Server 

Servicios de federación de Active Directory (AD FS) \( AD FS \) proporciona funciones de SSO de identidad simplificada y protegida e inicio de sesión único Web \- \( \) para usuarios finales que desean tener acceso a aplicaciones dentro de un AD FS \- empresa protegida, en organizaciones de asociados de Federación o en la nube.  
  
En Windows Server &reg; 2012 R2, AD FS incluye un servicio de rol de servicio de Federación que actúa como proveedor de identidades que \( autentica a los usuarios para proporcionar tokens de seguridad a las aplicaciones que confían AD FS \) o como proveedor \( de Federación consume tokens de otros proveedores de identidades y, a continuación, proporciona tokens de seguridad a las aplicaciones que confían AD FS \) .  
  
La función de proporcionar acceso a extranet para aplicaciones y servicios que están protegidos por AD FS en Windows Server 2012 R2 ahora se realiza mediante un nuevo servicio de rol de acceso remoto denominado Proxy de aplicación web. Se trata de un cambio respecto a las versiones anteriores de Windows Server en las que esta función se controlaba mediante un servidor proxy de federación AD FS. Proxy de aplicación web es un rol de servidor diseñado para proporcionar acceso para el \- escenario de extranet relacionado con el AD FS y otros escenarios de extranet. Para obtener más información sobre el proxy de aplicación Web, consulte [Guía de tutorial de proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11)).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
En esta guía se proporcionan recomendaciones para ayudarle a planear una nueva implementación de AD FS, en función de los requisitos de su organización. Esta guía está pensada para especialistas en infraestructura o arquitectos de sistemas. Resalta sus puntos de decisión principales a medida que planea la implementación de AD FS. Antes de leer esta guía, debe tener una buena comprensión de cómo funciona AD FS en un nivel funcional. Para obtener más información, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Identificar los objetivos de implementación de AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planear la topología de la implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisitos de AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Consulte también  
[Diseño de AD FS](../../ad-fs/AD-FS-Design.md)  
  
