---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: "Guía de implementación de Windows Server 2012 AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guía de implementación de Windows Server 2012 AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar servicios de federación de Active Directory® \(AD FS\) con el sistema operativo de Windows Server® 2012 para compilar una solución de administración de identidad federada que extiende identificación distribuido, autenticación y autorización servicios a aplicaciones basadas en Web\ a través de la organización y límites de la plataforma. Mediante la implementación de AD FS, puedes ampliar capacidades de administración de identidades existente de tu organización a Internet.  
  
Puedes implementar AD FS para:  
  
-   Proporcionan sus empleados o clientes una experiencia de \(SSO\) basados en Web\, single\ sign\ en cuando los necesitan acceso remoto a los servicios o sitios Web hospedado internamente.  
  
-   Proporcionar sus empleados o clientes una experiencia SSO basado en Web\ cuando tienen acceso a sitios Web cross\ organizativa o servicios desde los servidores de seguridad de la red.  
  
-   Proporcionar sus empleados o clientes con un acceso transparente a los recursos basados en Web\ en cualquier organización de partner de federación en Internet sin necesidad de que los empleados o clientes iniciar sesión más de una vez.  
  
-   Conservar el control total sobre las identidades de empleados o clientes sin usar otros proveedores sign\ en \ (Windows Live ID, libertad Alliance y others\).  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía está destinada para su uso por los administradores del sistema y los ingenieros de sistema. Proporciona instrucciones detalladas para implementar un diseño de AD FS que ha sido preseleccionado por usted o infraestructura especialista o sistema arquitecto en la organización.  
  
Si aún no se ha seleccionado un diseño, te recomendamos que esperes a seguir las instrucciones de esta guía hasta después de revisar las opciones de diseño de la [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) y ha seleccionado el diseño más adecuado para la organización. Para obtener más información sobre el uso de esta guía con un diseño que ya se ha seleccionado, consulta [implementar su Plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Después de seleccionar el diseño de la Guía de diseño y recopilar la información necesaria sobre las notificaciones, tipos de token, atributo tiendas y otros elementos, puedes usar a esta guía para implementar el diseño de AD FS en el entorno de producción. Esta guía proporciona los pasos para implementar cualquiera de los siguientes diseños de AD FS principales:  
  
-   SSO Web  
  
-   SSO Web federado  
  
Usar las listas de comprobación en [implementar su Plan de diseño de AD FS](Implementing-Your-AD-FS-Design-Plan.md) para determinar la mejor forma para usar las instrucciones que aparecen en esta guía para implementar el diseño particular. Para obtener información sobre los requisitos de hardware y software para la implementación de AD FS, vea la [Apéndice A: revisar AD FS requisitos](https://technet.microsoft.com/library/ff678034.aspx) en la Guía de diseño de AD FS.  
  
### <a name="what-this-guide-does-not-provide"></a>Lo que no proporciona esta guía  
No proporciona esta guía:  
  
-   Instrucciones sobre cuándo y dónde colocar los servidores de federación, proxies de servidor de federación o servidores Web en la infraestructura de red. Para obtener esta información, consulta [planeación colocación del servidor de federación](https://technet.microsoft.com/library/dd807069.aspx) y [planeación de colocación de Proxy del servidor de federación](https://technet.microsoft.com/library/dd807130.aspx) en la Guía de diseño de AD FS.  
  
-   Instrucciones para usar certificación entidades \(CAs\) para configurar AD FS  
  
-   Instrucciones para configurar o configurar aplicaciones específicas basadas en Web\  
  
-   Instrucciones que son específicas para configurar un entorno de laboratorio de prueba de la instalación.  
  
-   Información sobre cómo personalizar las pantallas de inicio de sesión federado, los archivos de web.config o la base de datos de configuración.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Planear la implementación de AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Implementar tu AD FS diseñar Plan](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Lista de comprobación: Implementar un diseño de inicio de sesión único Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de comprobación: Implementar un diseño SSO Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configuración de las organizaciones asociadas](Configuring-Partner-Organizations.md)  
  
-   [Configurar reglas de notificación](Configuring-Claim-Rules.md)  
  
-   [Implementación de los servidores de federación](Deploying-Federation-Servers.md)  
  
-   [Implementar el proxy de servidor de federación](Deploying-Federation-Server-Proxies.md)  
  
-   [Interoperar con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
