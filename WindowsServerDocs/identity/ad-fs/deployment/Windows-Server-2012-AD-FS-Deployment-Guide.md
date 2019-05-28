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
ms.openlocfilehash: 6be56c25cc6f639f73842f57cdf48a6339dccf9c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191854"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guía de implementación de AD FS en Windows Server 2012


Puede usar servicios de federación de Active Directory® \(AD FS\) con el sistema operativo Windows Server® 2012 para crear una solución de administración de identidades federadas que extiende la identificación distribuida, autenticación, y Servicios de autorización en Web\-en función de las aplicaciones a través de límites de la organización y la plataforma. Al implementar AD FS puede ampliar las capacidades existentes administración de identidades de su organización en Internet.  
  
Puede implementar AD FS para:  
  
-   Proporcionar a los empleados o clientes una Web\-basado, único\-sesión\-en \(SSO\) experiencia cuando necesiten acceso remoto a internamente hospeda sitios Web o servicios.  
  
-   Proporcionar a los empleados o clientes una Web\-basado, la experiencia de SSO cuando acceden a\-sitios Web de la organización o los servicios desde los servidores de seguridad de la red.  
  
-   Proporcionar los empleados o clientes con acceso ininterrumpido a Web\-basadas en recursos en cualquier organización del asociado de federación en Internet sin necesidad de que los empleados o clientes iniciar sesión varias veces.  
  
-   Mantener pleno control de las identidades de empleados o clientes sin usar otro signo\-en proveedores \(Windows Live ID, Liberty Alliance etc\).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía está pensada para administradores de sistemas e ingenieros de sistemas. Proporciona instrucciones detalladas para implementar un diseño de AD FS preseleccionado por usted o un arquitecto de sistema o especialista en infraestructura de su organización.  
  
Si aún no se ha seleccionado un diseño, se recomienda que espere a que siga las instrucciones de esta guía hasta después de revisar las opciones de diseño en el [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) y ha seleccionado el máximo diseño apropiado para su organización. Para obtener más información sobre el uso de esta guía con un diseño que ya se ha seleccionado, vea [implementar su Plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Después de seleccionar el diseño de la Guía de diseño y recopilar la información necesaria acerca de las notificaciones, tipos de token, almacenes de atributos y otros elementos, puede usar a esta guía para implementar un diseño de AD FS en su entorno de producción. Esta guía proporciona pasos para implementar uno de los diseños AD FS principales:  
  
-   Web SSO  
  
-   SSO web federado  
  
Use las listas de comprobación en [implementar su Plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md) para determinar la mejor manera de usar las instrucciones de esta guía para implementar un diseño determinado. Para obtener información acerca de los requisitos de hardware y software para implementar AD FS, consulte el [Apéndice A: Revisar los requisitos de AD FS](https://technet.microsoft.com/library/ff678034.aspx) en el diseño de AD FS guía.  
  
### <a name="what-this-guide-does-not-provide"></a>Qué no incluye esta guía  
Esta guía no proporciona:  
  
-   Orientación sobre cuándo y dónde colocar los servidores Web, servidores proxy de federación o servidores de federación en la infraestructura de red existente. Para obtener esta información, consulte [planear la ubicación de servidor de federación](https://technet.microsoft.com/library/dd807069.aspx) y [planear la ubicación del Proxy de servidor de federación](https://technet.microsoft.com/library/dd807130.aspx) en la Guía de diseño de AD FS.  
  
-   Instrucciones para usar entidades de certificación \(CAs\) para configurar AD FS  
  
-   Instrucciones para instalar o configurar Web específico\-aplicaciones basadas en  
  
-   Instrucciones de configuración específicas para configurar un entorno de laboratorio de prueba.  
  
-   Información sobre cómo personalizar pantallas de inicio de sesión federads, archivos web.config o la base de datos de configuración.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Planeación para la implementación de AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Implementación del plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Lista de comprobación: Implementar un diseño de SSO web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de comprobación: Implementar un diseño de SSO web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configuración de las organizaciones asociadas](Configuring-Partner-Organizations.md)  
  
-   [Configuración de reglas de notificación](Configuring-Claim-Rules.md)  
  
-   [Implementación de servidores de federación](Deploying-Federation-Servers.md)  
  
-   [Implementación de servidores proxy de federación](Deploying-Federation-Server-Proxies.md)  
  
-   [Interoperación con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
