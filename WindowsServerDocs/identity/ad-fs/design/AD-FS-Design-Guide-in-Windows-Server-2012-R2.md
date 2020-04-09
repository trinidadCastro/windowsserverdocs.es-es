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
ms.openlocfilehash: e8675c255032bca4623a9649bfc0bcca478008e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854898"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guía de diseño de AD FS en Windows Server 

Servicios de federación de Active Directory (AD FS) \(AD FS\) proporciona la Federación de identidades simplificada y protegida y\-de inicio de sesión Web en \(características de SSO\) para los usuarios finales que desean tener acceso a las aplicaciones dentro de un AD FS\-una empresa protegida, en organizaciones de asociados de Federación o en la nube.  
  
En Windows Server&reg; 2012 R2, AD FS incluye un servicio de rol de servicio de Federación que actúa como proveedor de identidad \(autentica a los usuarios para proporcionar tokens de seguridad a las aplicaciones que confían AD FS\) o como proveedor de Federación \(consume tokens de otros proveedores de identidades y, a continuación, proporciona tokens de seguridad a las aplicaciones que confían AD FS\).  
  
La función de proporcionar acceso a extranet para aplicaciones y servicios que están protegidos por AD FS en Windows Server 2012 R2 ahora se realiza mediante un nuevo servicio de rol de acceso remoto denominado Proxy de aplicación web. Se trata de un cambio respecto a las versiones anteriores de Windows Server en el que esta función se controlaba mediante un servidor proxy de federación de AD FS. Proxy de aplicación web es un rol de servidor diseñado para proporcionar acceso para el AD FS\-escenario de extranet relacionado y otros escenarios de extranet. Para obtener más información sobre el proxy de aplicación Web, consulte [Guía de tutorial de proxy de aplicación web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
En esta guía se proporcionan recomendaciones para ayudarle a planear una nueva implementación de AD FS, en función de los requisitos de su organización. Esta guía está pensada para especialistas en infraestructura o arquitectos de sistemas. Resalta sus puntos de decisión principales a medida que planea la implementación de AD FS. Antes de leer esta guía, debe tener una buena comprensión de cómo funciona AD FS en un nivel funcional. Para obtener más información, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Identificar los objetivos de implementación de AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planear la topología de la implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisitos de AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Consulta también  
[Diseño de AD FS](../../ad-fs/AD-FS-Design.md)  
  

