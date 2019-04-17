---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: "Guía de diseño de AD FS en Windows Server 2012"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>Guía de diseño de AD FS en Windows Server 2012

>Se aplica a: Windows Server 2012
  
> [!NOTE]  
> Para obtener información sobre cómo implementar AD FS en Windows Server 2012 R2, consulta [Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Puedes usar servicios de federación de Active Directory® \(AD FS\) con el sistema operativo de Windows Server® 2012 en una función de proveedor de servicios de federación autenticarse sin problemas a los usuarios de los servicios basados en Web\ o aplicaciones que residen en una organización de partner de recursos, sin necesidad de que los administradores crear o mantener confianzas externas o confianzas de bosque entre las redes de ambas organizaciones y sin la necesidad de que los usuarios iniciar sesión otra vez. El proceso de autenticación en una red al obtener acceso a recursos en otra red, sin la carga de las acciones de inicio de sesión repetidas que los usuarios, se conoce como el único sign\ en \(SSO\).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía proporcionan recomendaciones que te ayudarán a planear una implementación de AD FS, en función de los requisitos de la organización \ (también denominado en esta guía de implementación goals\) y el diseño particular que quieres crear. Esta guía está destinada al arquitecto especialista o el sistema de infraestructura. Resalta los puntos de principal decisiones al planear la implementación de AD FS. Antes de leer a esta guía, debe tener una buena comprensión de cómo funciona la AD FS en un nivel funcional. También debe tener una buena comprensión de los requisitos de la organización que se reflejarán en el diseño de AD FS.  
  
Esta guía describe un conjunto de objetivos de la implementación que se basan en los tres diseños de AD FS principales, y le ayuda a decidir el diseño más adecuado para el entorno. Puedes usar estos objetivos de implementación para formar uno de los siguientes diseños de AD FS completos o un diseño personalizado que satisfaga las necesidades de su entorno:  
  
-   Federados SSO en Web para admitir escenarios \(B2B\) business\-to\-business y para admitir la colaboración entre empresas con bosques independientes  
  
-   SSO para admitir el acceso de cliente a las aplicaciones en escenarios \(B2C\) business\-to\-consumer Web  
  
Para cada diseño, encontrarás instrucciones para recopilar los datos necesarios acerca de su entorno. A continuación, puedes usar estas directrices para planear y diseñar la implementación de AD FS. Después de leer esta guía y finalizar la recopilación, documentar y asignación de requisitos de la organización, tendrás la información necesaria comenzar a implementar AD FS mediante las instrucciones en el [Guía de implementación de Windows Server 2012 AD FS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Identificar los objetivos de implementación de AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Asignar los objetivos de la implementación a un diseño de AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinar la topología de implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Planear la implementación](Planning-Your-Deployment.md)  
  
-   [Colocación del servidor de federación de planeación](Planning-Federation-Server-Placement.md)  
  
-   [Colocación del servidor Proxy de federación de planeación](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Planear la capacidad de AD FS Server](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Apéndice A: revisar los requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

