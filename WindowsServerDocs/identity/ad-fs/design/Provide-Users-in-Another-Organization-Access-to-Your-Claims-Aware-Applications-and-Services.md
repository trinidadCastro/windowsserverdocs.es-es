---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: "Proporcionar a los usuarios en otra organización acceso a los servicios y las aplicaciones de notificaciones"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Proporcionar a los usuarios en otra organización acceso a los servicios y las aplicaciones de notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando es un administrador en la organización de partner de recursos en los servicios de federación de Active Directory \(AD FS\) y tiene un objetivo de implementación para proporcionar acceso federado para los usuarios en otra organización \ (organization\ de partner de cuenta) a una aplicación con reconocimiento de claims\ o un servicio basado en Web\ que se encuentra en la organización \ (organization\ de partner de recursos):  
  
-   En las organizaciones que han configurado una federación y en la organización de confianza en la organización de los usuarios federados \ (cuenta partner organizations\) pueden tener acceso a AD FS protegida la aplicación o servicio que está hospedado por su organización. Para obtener más información, consulta [federados Web SSO diseño](Federated-Web-SSO-Design.md).  
  
    Por ejemplo, Fabrikam puede sus empleados de la red corporativa tengan acceso a servicios Web que se hospedan en Contoso federado.  
  
-   Los usuarios que no tienen ninguna relación directa con una organización de confianza federados \ (por ejemplo, customers\ individuales), que han iniciado sesión en un almacén de atributo que está hospedado en la red perimetral, puede acceder a varias protege AD FS\ aplicaciones, que también se hospedan en su red perimetral, iniciando sesión una vez desde los equipos cliente que se encuentran en Internet. En otras palabras, cuando hospedar las cuentas de cliente para habilitar el acceso a aplicaciones o servicios en la red perimetral, los clientes que se hospedan en un almacén de atributo pueden acceder a una o más aplicaciones o servicios en la red perimetral simplemente al inicio de sesión. Para obtener más información, consulta [Web SSO diseño](Web-SSO-Design.md).  
  
    Por ejemplo, Fabrikam puede sus clientes single\ sign\ en \(SSO\) acceder a varias aplicaciones o servicios que se hospedan en su red perimetral.  
  
Los siguientes componentes son necesarios para este objetivo de implementación:  
  
-   **Active \(AD DS\) directorio los servicios de dominio:** el servidor de federación de asociado de recursos debe estar unido a un dominio de Active Directory.  
  
-   **Perimetral DNS:** dominio sistema de nombres \(DNS\) debe contener un registro de host simple \(A\) recursos para que los equipos cliente pueden encontrar el servidor de federación de asociado de recursos y el servidor Web. El servidor DNS puede hospedar otros registros DNS que también son necesarios en la red de perímetro. Para obtener más información, consulta [requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de federación de asociado de recurso:** el servidor de federación de recursos asociado valida los tokens de AD FS que enviar los socios de cuenta. Detección de asociado de cuenta se realiza a través de este servidor de federación. Para obtener más información, consulta [revisar el rol de servidor de federación de asociado de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Servidor Web:** que el servidor Web puede hospedar una aplicación Web o un servicio Web. El servidor Web confirma que recibe los tokens de AD FS válidos de los usuarios federados antes de permitir el acceso al servicio Web o aplicación Web protegido.  
  
    Al usar Windows Identity Foundation \(WIF\), puedes desarrollar tu aplicación o servicio Web para que acepte solicitudes de inicio de sesión de usuario federado que se realizan con cualquier método de inicio de sesión estándar, como el nombre de usuario y contraseña.  
  
Después de revisar la información de los temas vinculados, puedes empezar a implementar este objetivo siguiendo los pasos de [lista de comprobación: implementar un diseño de inicio de sesión único Web federados](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) y [lista de comprobación: implementar un diseño de inicio de sesión único Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
La siguiente ilustración muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.  
  
![Acceso a las reclamaciones](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
