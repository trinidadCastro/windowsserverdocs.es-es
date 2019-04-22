---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guía de diseño de AD FS en Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822156"
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>Guía de diseño de AD FS en Windows Server 2012 R2

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Servicios de federación de Active Directory \(AD FS\) proporciona la federación de identidades simplificada y protegida e inicio de sesión único Web\-en \(SSO\) capacidades para los usuarios finales que desean tener acceso a las aplicaciones dentro de AD FS\-protege la empresa, en organizaciones de asociados de federación o en la nube.  
  
En Windows Server® 2012 R2, AD FS incluye un servicio de rol Servicio de federación que actúa como proveedor de identidades \(autentica a los usuarios para proporcionar tokens de seguridad a las aplicaciones que confían en AD FS\) o como un proveedor de federación \( consume tokens de otros proveedores de identidades y, a continuación, proporciona tokens de seguridad a las aplicaciones que confían en AD FS\).  
  
La función de proporcionar acceso a extranet para aplicaciones y servicios que están protegidos por AD FS en Windows Server 2012 R2 ahora se realiza mediante un nuevo servicio de rol de acceso remoto denominado Proxy de aplicación web. Se trata de un cambio respecto a las versiones anteriores de Windows Server en el que esta función se controlaba mediante un servidor proxy de federación de AD FS. Proxy de aplicación Web es un rol de servidor diseñado para proporcionar acceso a la instancia de AD FS\-relacionados con el escenario de extranet y otros escenarios de extranet. Para obtener más información sobre el Proxy de aplicación Web, consulte [Guía de tutorial de Proxy de aplicación Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía proporciona recomendaciones para ayudarle a planear una nueva implementación de AD FS, según los requisitos de su organización. Esta guía está pensada para especialistas en infraestructura o arquitectos de sistemas. Resalta sus puntos de decisión principales cuando planee la implementación de AD FS. Antes de leer a esta guía, debe tener una buena comprensión del funcionamiento de AD FS en un nivel funcional. Para obtener más información, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Identificar los objetivos de implementación de AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planear la topología de implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisitos de AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Vea también  
[Diseño de AD FS](../../ad-fs/AD-FS-Design.md)  
  

