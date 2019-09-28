---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guía de diseño de AD FS en Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 614bc2b4571dd8a1b35c075ae1dec6934e77e148
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408162"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guía de diseño de AD FS en Windows Server 

Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 proporciona la Federación de identidades simplificada y protegida, y las funcionalidades de firma única web @ no__t-2on \(SSO @ no__t-4 para los usuarios finales que desean tener acceso a las aplicaciones dentro de un AD FS @ no__t-5secured Enterprise, en organizaciones de asociados de Federación o en la nube.  
  
En Windows Server® 2012 R2, AD FS incluye un servicio de rol de servicio de Federación que actúa como proveedor de identidades \(authenticates los usuarios para proporcionar tokens de seguridad a las aplicaciones que confían AD FS @ no__t-1 o como proveedor de Federación \(consumes tokens de otros proveedores de identidades y, a continuación, proporciona tokens de seguridad a las aplicaciones que confían AD FS @ no__t-3.  
  
La función de proporcionar acceso a extranet para aplicaciones y servicios que están protegidos por AD FS en Windows Server 2012 R2 ahora se realiza mediante un nuevo servicio de rol de acceso remoto denominado Proxy de aplicación web. Se trata de un cambio respecto a las versiones anteriores de Windows Server en el que esta función se controlaba mediante un servidor proxy de federación de AD FS. Proxy de aplicación web es un rol de servidor diseñado para proporcionar acceso para el escenario de extranet de AD FS @ no__t-0related y otros escenarios de extranet. Para obtener más información sobre el proxy de aplicación Web, consulte [Guía de tutorial de proxy de aplicación web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
En esta guía se proporcionan recomendaciones para ayudarle a planear una nueva implementación de AD FS, en función de los requisitos de su organización. Esta guía está pensada para especialistas en infraestructura o arquitectos de sistemas. Resalta sus puntos de decisión principales a medida que planea la implementación de AD FS. Antes de leer esta guía, debe tener una buena comprensión de cómo funciona AD FS en un nivel funcional. Para obtener más información, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Identificar los objetivos de implementación de AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planear la topología de la implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisitos de AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Vea también  
[Diseño de AD FS](../../ad-fs/AD-FS-Design.md)  
  

