---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Diseño de SSO web federado
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9915a2942c9336d5aeb7776169d2e51491c22909
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853148"
---
# <a name="federated-web-sso-design"></a>Diseño de SSO web federado

El inicio de sesión\-único Web federado\-en \(diseño de\) SSO en Servicios de federación de Active Directory (AD FS) \(AD FS\) implica una comunicación segura que abarca varios firewalls, redes perimetrales y servidores de resolución de nombres\-, además de toda la infraestructura de enrutamiento de Internet.  
  
Normalmente, este diseño se utiliza cuando dos organizaciones acuerdan crear una relación de confianza de Federación para permitir a los usuarios de una organización \(la organización del asociado de cuenta\) acceder a aplicaciones o servicios basados en Web\-, que están protegidos por AD FS, en la otra organización \(la organización del asociado de recurso\).  
  
En otras palabras, una relación de confianza de Federación es la encarnación de un acuerdo de nivel de\-de negocio o una asociación entre dos organizaciones. Como se muestra en la siguiente ilustración, puede establecer una relación de confianza de Federación entre dos empresas, lo que da como resultado una\-final a\-escenario de Federación.  
  
![SSO Web federado](media/adfs2_FederatedWebSSODesign.gif)  
  
La flecha de una\-forma de la ilustración indica la dirección de la confianza de Federación, que, como la dirección de las confianzas de Windows, siempre señala al lado de la cuenta del bosque. Esto significa que la autenticación fluye desde la organización del asociado de cuenta a la organización del asociado de recurso.  
  
En este diseño de SSO Web federado, dos servidores de Federación \(uno en Fabrikam y el otro en Contoso\) enrutar las solicitudes de autenticación de las cuentas de usuario de Fabrikam a aplicaciones o servicios basados en Web\-en contoso.  
  
> [!NOTE]  
> Para obtener seguridad adicional, puede usar servidores proxy de Federación para retransmitir las solicitudes a los servidores de Federación que no son accesibles directamente desde Internet.  
  
En este ejemplo, Fabrikam es el proveedor de identidad o cuenta. La parte de Fabrikam del diseño de SSO Web federado usa el siguiente objetivo de implementación de AD FS:  
  
-   [Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso es el proveedor de recursos. La parte de Contoso del diseño de SSO Web federado logra los siguientes objetivos de implementación de AD FS:  
  
-   [Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obtener una lista de tareas detalladas que puedes utilizar para planear e implementar el diseño de SSO web, consulta [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
