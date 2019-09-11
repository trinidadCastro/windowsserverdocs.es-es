---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Guía de implementación de AD FS en Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 06946942bcdc5ea00acc22b91d6551a826357fcf
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868027"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guía de implementación de AD FS en Windows Server 2012


Puede usar Active Directory® servicios \(de Federación AD FS\) con el sistema operativo Windows Server® 2012 para crear una solución de administración de identidades federada que amplíe la identificación distribuida, la autenticación y servicios de autorización para\-aplicaciones basadas en Web a través de los límites de la organización y la plataforma. Mediante la implementación de AD FS, puede ampliar las capacidades existentes de administración de identidades de su organización a Internet.  
  
Puede implementar AD FS para:  
  
-   Proporcionar a los empleados o clientes una experiencia\-de SSO\) de inicio\-de \(sesión único\-basada en Web cuando necesiten acceso remoto a sitios o servicios web hospedados internamente.  
  
-   Proporcionar a los empleados o clientes una experiencia\-de SSO basada en Web cuando accedan\-a sitios web o servicios entre organizaciones desde los firewalls de la red.  
  
-   Proporcionar a los empleados o clientes un acceso sin problemas\-a los recursos basados en Web en cualquier organización asociada de Federación en Internet sin necesidad de que los empleados o clientes inicien sesión más de una vez.  
  
-   Conserve el control total sobre las identidades de los empleados o clientes\-sin usar \(otros proveedores de inicio de sesión Windows Live ID\), Liberty Alliance, etc.  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía está pensada para administradores de sistemas e ingenieros de sistemas. Proporciona instrucciones detalladas para implementar un diseño de AD FS que ha sido preseleccionado por usted o por un especialista en infraestructuras o un arquitecto de sistemas de su organización.  
  
Si todavía no se ha seleccionado un diseño, se recomienda esperar a seguir las instrucciones de esta guía hasta después de revisar las opciones de diseño de la guía de [diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) y haber seleccionado el diseño más apropiado para su Organización. Para obtener más información sobre el uso de esta guía con un diseño que ya se ha seleccionado, vea [implementar el plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Después de seleccionar el diseño de la guía de diseño y de recopilar la información necesaria acerca de las notificaciones, los tipos de token, los almacenes de atributos y otros elementos, puede usar esta guía para implementar el diseño de AD FS en el entorno de producción. En esta guía se proporcionan los pasos para implementar cualquiera de los siguientes diseños de AD FS principales:  
  
-   Web SSO  
  
-   SSO web federado  
  
Use las listas de comprobación para [implementar el plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md) para determinar el mejor modo de utilizar las instrucciones de esta guía para implementar su diseño determinado. Para obtener información acerca de los requisitos de hardware y software para implementar AD FS [, consulte el Apéndice A: Revisar los requisitos](https://technet.microsoft.com/library/ff678034.aspx) de AD FS en la guía de diseño de AD FS.  
  
### <a name="what-this-guide-does-not-provide"></a>Qué no incluye esta guía  
Esta guía no proporciona:  
  
-   Orientación sobre cuándo y dónde colocar los servidores de Federación, los servidores proxy de Federación o los servidores Web en la infraestructura de red existente. Para obtener esta información, consulte planeamiento de la ubicación del [servidor de Federación](https://technet.microsoft.com/library/dd807069.aspx) y [planeación](https://technet.microsoft.com/library/dd807130.aspx) de la colocación del servidor proxy de Federación en la guía de diseño de AD FS.  
  
-   Instrucciones para el uso de \(entidades\) de certificación CA para configurar AD FS  
  
-   Guía para la instalación o configuración de aplicaciones\-específicas basadas en Web  
  
-   Instrucciones de configuración específicas para configurar un entorno de laboratorio de prueba.  
  
-   Información sobre cómo personalizar pantallas de inicio de sesión federads, archivos web.config o la base de datos de configuración.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Planeación para la implementación de AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Implementación del plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Lista de comprobación: Implementar un diseño de SSO web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de comprobación: Implementar un diseño de SSO web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configuración de organizaciones asociadas](Configuring-Partner-Organizations.md)  
  
-   [Configuración de reglas de notificación](Configuring-Claim-Rules.md)  
  
-   [Implementación de servidores de federación](Deploying-Federation-Servers.md)  
  
-   [Implementación de servidores proxy de federación](Deploying-Federation-Server-Proxies.md)  
  
-   [Interoperación con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
