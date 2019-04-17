---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: "Guía de diseño de AD FS en Windows Server 2012 R2"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>Guía de diseño de AD FS en Windows Server 2012 R2

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Active \(AD FS\) servicios de federación Directory proporciona federación de identidades simplificada, protegido y Web solo en sign\ \(SSO\) funcionalidades para los usuarios finales que quieran tener acceso a aplicaciones dentro de una empresa protege FS\ anuncios, en las organizaciones asociadas de federación o en la nube.  
  
En Windows Server® 2012 R2, AD FS incluye un servicio de rol de servicios de federación que actúa como un proveedor de identidad \ (autentica a los usuarios para proporcionar los tokens de seguridad a las aplicaciones que confía FS\ AD) o como un proveedor de federación \ (consume tokens de otros proveedores de identidad y, a continuación, proporciona los tokens de seguridad a las aplicaciones que confía en AD FS\).  
  
La función de proporcionar acceso a la extranet a aplicaciones y servicios que están protegidos por AD FS en Windows Server 2012 R2 realiza ahora un nuevo servicio de rol de acceso remoto denominado a Proxy de aplicación Web. Este es un cambio respecto a las versiones anteriores de Windows Server en el que esta función se controla por un proxy de servidor de federación de AD FS. Proxy de aplicación Web está diseñado para proporcionar acceso para el escenario de extranet relacionados con AD FS\ y otros escenarios extranet un rol de servidor. Para obtener más información sobre el Proxy de aplicación Web, consulta [Proxy tutorial guía de aplicaciones de Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía proporciona recomendaciones que te ayudarán a planear una implementación de AD FS, en función de los requisitos de la organización. Esta guía está destinada al arquitecto especialista o el sistema de infraestructura. Resalta los puntos de principal decisiones al planear la implementación de AD FS. Antes de leer a esta guía, debe tener una buena comprensión de cómo funciona la AD FS en un nivel funcional. Para obtener más información, consulta [comprensión clave AD FS conceptos](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Identificar los objetivos de implementación de AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planear la topología de implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisitos de AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Consulta también  
[Diseño de AD FS](../../ad-fs/AD-FS-Design.md)  
  

