---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: "Diseño SSO Web federado"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b85f49ac0556bf9b3542a23514d7fcbf82d2d88e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="federated-web-sso-design"></a>Diseño SSO Web federado

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El diseño de federados Web Single\-Sign\-On \(SSO\) en los servicios de federación de Active Directory \(AD FS\) incluyan la comunicación segura que abarque varias firewalls, redes perimetrales y servidores de resolución equipo\, además de toda la infraestructura de enrutamiento de Internet.  
  
Por lo general, este diseño se usa cuando dos organizaciones acepta crear una relación de confianza de federación para permitir que los usuarios de una organización \ (la cuenta partner organization\) para obtener acceso a aplicaciones basadas en Web\ o servicios, que están protegidos por AD FS, en la otra organización \ (organization\ de partner de recursos).  
  
En otras palabras, una relación de confianza de federación es la manifestación de un contrato de nivel de business\ o asociación entre dos organizaciones. Como se muestra en la siguiente ilustración, puedes establecer una relación de confianza de federación entre dos empresas, lo que resulta en un escenario de federación de end\-to\-end.  
  
![sso web federado](media/adfs2_FederatedWebSSODesign.gif)  
  
La flecha one\ vías en la ilustración indica la dirección de la federación de confianza, como la dirección de confianzas de Windows: siempre apunta hacia el lado de la cuenta del bosque. Esto significa que la autenticación de flujos de la organización de cuenta asociada a la organización de partner de recurso.  
  
En este diseño SSO Web federado, dos servidores de federación \ (uno en Fabrikam y el otro en Contoso\) enrutan solicitudes de autenticación de cuentas de usuario de Fabrikam Web\ aplicaciones o los servicios de Contoso.  
  
> [!NOTE]  
> Para mayor seguridad, puedes usar a servidores proxy de servidor de federación para transmitir solicitudes a los servidores de federación que no sean accesibles directamente desde Internet.  
  
En este ejemplo, Fabrikam es el proveedor de identidad o cuenta. La parte de Fabrikam del diseño SSO Web federado usa el siguiente objetivo de implementación de AD FS:  
  
-   [Proporcionar el acceso a los usuarios de Active Directory para las aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso es el proveedor de recursos. La parte de Contoso del diseño SSO Web federado logra los siguientes objetivos de implementación de AD FS:  
  
-   [Proporcionar a los usuarios en otra organización acceso a los servicios y las aplicaciones de notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Proporcionar el acceso a los usuarios de Active Directory a los servicios y las aplicaciones de notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obtener una lista de las tareas detalladas que puedes usar para planear e implementar el diseño SSO Web federado, consulta [lista de comprobación: implementar un diseño de inicio de sesión único Web federados ](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
