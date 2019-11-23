---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guía de diseño de AD FS en Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d9d7ec6f4ff575d3aac30b7127e591b78f5ef49b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359212"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guía de diseño de AD FS en Windows Server 


  
> [!NOTE]  
> Para obtener información sobre cómo implementar AD FS en Windows Server 2012 R2, consulte la [Guía de implementación de Windows server 2012 r2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Puede usar Active Directory servicios de Federación de® \(AD FS\) con el sistema operativo Windows Server® 2012 en un rol de proveedor de servicios de Federación para autenticar sin problemas a los usuarios en cualquier servicio Web\-o aplicación que resida en una organización de asociado de recurso. sin la necesidad de que los administradores creen o mantengan confianzas externas o confianzas de bosque entre las redes de ambas organizaciones y sin necesidad de que los usuarios inicien sesión por segunda vez. El proceso de autenticación en una red a la vez que se accede a los recursos de otra red, sin la carga de las acciones de inicio de sesión repetidas por los usuarios, se conoce como\-de inicio de sesión único en \(\)SSO.  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
En esta guía se proporcionan recomendaciones para ayudarle a planear una nueva implementación de AD FS, en función de los requisitos de su organización \(también se hace referencia a ellos en esta guía como objetivos de implementación\) y el diseño concreto que desea crear. Esta guía está pensada para especialistas en infraestructura o arquitectos de sistemas. Resalta sus puntos de decisión principales a medida que planea la implementación de AD FS. Antes de leer esta guía, debe tener una buena comprensión de cómo funciona AD FS en un nivel funcional. También debe tener una buena comprensión de los requisitos de la organización que se reflejarán en el diseño de AD FS.  
  
En esta guía se describe un conjunto de objetivos de implementación que se basan en tres diseños de AD FS principales y le ayuda a decidir el diseño más apropiado para su entorno. Puede usar estos objetivos de implementación para crear uno de los siguientes diseños de AD FS completos o un diseño personalizado que satisfaga las necesidades de su entorno:  
  
-   SSO Web federado para admitir\-empresariales para\-escenarios empresariales \(B2B\) y para admitir la colaboración entre unidades de negocio con bosques independientes  
  
-   SSO Web para admitir el acceso del cliente a las aplicaciones de la\-empresarial para\-los escenarios\) de consumidor \(B2C  
  
Para cada diseño, encontrarás directrices para recopilar los datos necesarios acerca de tu entorno. Después, puede usar estas instrucciones para planear y diseñar la implementación de AD FS. Después de leer esta guía y finalizar la recopilación, la documentación y la asignación de los requisitos de su organización, tendrá la información necesaria para empezar a implementar AD FS siguiendo las instrucciones de la [Guía de implementación de Windows Server 2012 AD FS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Identificar los objetivos de implementación de AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Asignar los objetivos de implementación a un diseño de AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinar la topología de la implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Planear la implementación](Planning-Your-Deployment.md)  
  
-   [Planear la ubicación del servidor de federación](Planning-Federation-Server-Placement.md)  
  
-   [Planear la ubicación del servidor proxy de federación](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Planear la capacidad de los servidores de AD FS](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Apéndice A: revisión de los requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

