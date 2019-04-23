---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guía de diseño de AD FS en Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890216"
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>Guía de diseño de AD FS en Windows Server 2012

>Se aplica a: Windows Server 2012
  
> [!NOTE]  
> Para obtener información sobre cómo implementar AD FS en Windows Server 2012 R2, consulte [la Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Puede usar servicios de federación de Active Directory® \(AD FS\) con el equipo con Windows Server® 2012 del sistema operativo en una federación de servicios de rol de proveedor para autenticar sin problemas a los usuarios a cualquier Web\-servicios basados en o aplicaciones que residen en una organización del asociado de recurso, sin necesidad de que los administradores crear o mantener las confianzas externas o confianzas de bosque entre las redes de ambas organizaciones y sin necesidad de que los usuarios inicien sesión en una segunda vez. El proceso de autenticación en una red al tener acceso a los recursos de otra red, sin la carga de las acciones de inicio de sesión repetidas por los usuarios, se conoce como inicio de sesión único\-en \(SSO\).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía proporcionan recomendaciones para ayudarle a planear una nueva implementación de AD FS, según los requisitos de su organización \(también mencionados en esta guía como objetivos de implementación\) y el diseño concreto que desea crear. Esta guía está pensada para especialistas en infraestructura o arquitectos de sistemas. Resalta sus puntos de decisión principales cuando planee la implementación de AD FS. Antes de leer a esta guía, debe tener una buena comprensión del funcionamiento de AD FS en un nivel funcional. También debe tener una buena comprensión de los requisitos organizativos que se reflejarán en el diseño de AD FS.  
  
Esta guía describe un conjunto de objetivos de implementación que se basan en tres diseños AD FS principales, y le ayuda a decidir el diseño más apropiado para su entorno. Puede usar estos objetivos de implementación para crear uno de los diseños AD FS completos o un diseño personalizado que satisfaga las necesidades de su entorno:  
  
-   Federated Web SSO para admitir negocios\-a\-business \(B2B\) escenarios y mejore la colaboración entre unidades de negocio con bosques independientes.  
  
-   Web de inicio de sesión único para admitir el acceso de cliente a las aplicaciones de negocio\-a\-consumidor \(B2C\) escenarios  
  
Para cada diseño, encontrarás directrices para recopilar los datos necesarios acerca de tu entorno. A continuación, puede utilizar estas directrices para planear y diseñar la implementación de AD FS. Después de leer esta guía y finalizar la recopilación, documentar y asignar los requisitos de su organización, tendrá la información necesaria para comenzar a implementar AD FS mediante las instrucciones de la [Guía de implementación de Windows Server 2012 AD FS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Identificar los objetivos de implementación de AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Asignar los objetivos de implementación a un diseño de AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinar la topología de implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Planear la implementación](Planning-Your-Deployment.md)  
  
-   [Planear la ubicación del servidor de federación](Planning-Federation-Server-Placement.md)  
  
-   [Planear la ubicación de Proxy de servidor de federación](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Planear la capacidad de AD FS Server](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Apéndice A: Revisar los requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

