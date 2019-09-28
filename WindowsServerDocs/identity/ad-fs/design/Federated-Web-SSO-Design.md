---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Diseño de SSO web federado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a3e7eb6c42c8190da799c88c1e947e6aef1c29f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408109"
---
# <a name="federated-web-sso-design"></a>Diseño de SSO web federado

El diseño único Web federado @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-5 implica una comunicación segura que abarca varios firewalls, redes perimetrales y Name @ no__t-6resolution servidores: además de toda la infraestructura de enrutamiento de Internet.  
  
Normalmente, este diseño se utiliza cuando dos organizaciones acuerdan crear una relación de confianza de Federación para permitir a los usuarios de una organización @no__t la organización del asociado de cuenta de 0The @ no__t-1 para tener acceso a aplicaciones o servicios web @ no__t-2based, protegidos por AD FS, en la otra organización \(The de la organización del asociado de recurso @ no__t-4.  
  
En otras palabras, una relación de confianza de Federación es la encarnación de un acuerdo de negocio @ no__t-0level o una asociación entre dos organizaciones. Como se muestra en la siguiente ilustración, puede establecer una relación de confianza de Federación entre dos empresas, lo que da como resultado un escenario de Federación end @ no__t-0to @ no__t-1Fin.  
  
![SSO Web federado](media/adfs2_FederatedWebSSODesign.gif)  
  
La flecha @ no__t-0way de la ilustración indica la dirección de la confianza de Federación, que, como la dirección de las confianzas de Windows, siempre señala al lado de la cuenta del bosque. Esto significa que la autenticación fluye desde la organización del asociado de cuenta a la organización del asociado de recurso.  
  
En este diseño de SSO Web federado, dos servidores de Federación \(one en Fabrikam y el otro en Contoso @ no__t-1 redirigen las solicitudes de autenticación de las cuentas de usuario de Fabrikam a web @ no__t-2based aplicaciones o servicios de contoso.  
  
> [!NOTE]  
> Para obtener seguridad adicional, puede usar servidores proxy de Federación para retransmitir las solicitudes a los servidores de Federación que no son accesibles directamente desde Internet.  
  
En este ejemplo, Fabrikam es el proveedor de identidad o cuenta. La parte de Fabrikam del diseño de SSO Web federado usa el siguiente objetivo de implementación de AD FS:  
  
-   [Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso es el proveedor de recursos. La parte de Contoso del diseño de SSO Web federado logra los siguientes objetivos de implementación de AD FS:  
  
-   [Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obtener una lista de tareas detalladas que puede usar para planear e implementar el diseño de SSO Web federado, consulte @no__t 0Checklist: Implementación de un diseño de SSO Web federado @ no__t-0.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
