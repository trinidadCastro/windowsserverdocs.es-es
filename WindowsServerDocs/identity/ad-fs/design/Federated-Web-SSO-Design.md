---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Diseño de SSO web federado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b85f49ac0556bf9b3542a23514d7fcbf82d2d88e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865146"
---
# <a name="federated-web-sso-design"></a>Diseño de SSO web federado

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Federated Web único\-inicio de sesión\-en \(SSO\) diseño en servicios de federación de Active Directory \(AD FS\) supone una comunicación segura que abarque varios firewalls perimetrales las redes y el nombre\-servidores de resolución, además de toda la infraestructura de enrutamiento de Internet.  
  
Normalmente, este diseño se utiliza cuando dos organizaciones acuerdan crear una relación de confianza de federación para permitir que los usuarios de una organización \(la organización del asociado de cuenta\) para tener acceso a Web\-en función de las aplicaciones o servicios , que están protegidos por AD FS, en la otra organización \(la organización del asociado de recurso\).  
  
En otras palabras, una relación de confianza de federación es la encarnación de un negocio\-acuerdo de nivel o sociedad entre dos organizaciones. Como se muestra en la siguiente ilustración, puede establecer una relación de confianza de federación entre dos empresas, lo que da como resultado un end\-a\-escenario de federación.  
  
![sso web federado](media/adfs2_FederatedWebSSODesign.gif)  
  
Lo\-flecha de la forma en la ilustración representa la dirección de la federación de confianza, que, como la dirección de confianzas de Windows, siempre señala al lado de la cuenta del bosque. Esto significa que la autenticación fluye desde la organización del asociado de cuenta a la organización del asociado de recurso.  
  
En este diseño SSO Web federado, dos servidores de federación \(uno en Fabrikam y otro en Contoso\) enrutar las solicitudes de autenticación de cuentas de usuario de Fabrikam Web\-en función de las aplicaciones o servicios en Contoso.  
  
> [!NOTE]  
> Para mayor seguridad, puede utilizar a servidores proxy de federación para retransmitir solicitudes a servidores de federación que no son directamente accesibles desde Internet.  
  
En este ejemplo, Fabrikam es el proveedor de identidad o cuenta. La parte de Fabrikam del diseño SSO Web federado usa el siguiente objetivo de implementación de AD FS:  
  
-   [Proporcionar acceso de los usuarios de Active Directory a las aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso es el proveedor de recursos. La parte de Contoso del diseño SSO Web federado logra los objetivos de implementación de AD FS siguientes:  
  
-   [Proporcionar a los usuarios de otra organización acceso a los servicios y aplicaciones para notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Proporcionar acceso de los usuarios de Active Directory a los servicios y aplicaciones para notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obtener una lista de tareas detalladas que puede usar para planear e implementar el diseño SSO Web federado, vea [lista de comprobación: Implementar un diseño de SSO Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
