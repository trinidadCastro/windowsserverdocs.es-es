---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836536"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando es un administrador en la organización del asociado de recurso en Active Directory Federation Services \(AD FS\) y tiene un objetivo de implementación para proporcionar acceso federado para los usuarios de otra organización \(el organización del asociado de cuenta\) a una de las notificaciones\-una Web o aplicación compatible con\-en función de servicio que se encuentra en su organización \(la organización del asociado de recurso\):  
  
-   Los usuarios de su organización y de las organizaciones que han configurado una federación de confían en su organización federados \(las organizaciones de asociados de cuenta\) puede tener acceso a AD FS, aplicación o servicio protegido que está hospedado en su organización. Para obtener más información, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Por ejemplo, es posible que Fabrikam desee que sus empleados de la red corporativa tengan acceso federado a servicios web que se hospedan en Contoso.  
  
-   Los usuarios federados que no tienen ninguna asociación directa con una organización de confianza \(como clientes individuales\), que están conectados a un almacén de atributos que se hospeda en la red perimetral, puede tener acceso a varios AD FS\- aplicaciones protegidas, que también están hospedadas en la red perimetral, iniciando sesión una vez desde los equipos cliente que se encuentran en Internet. En otras palabras, cuando se hospedan cuentas de clientes para habilitar el acceso a aplicaciones o servicios de la red perimetral, los clientes que se hospedan en un almacén de atributos pueden tener acceso a una o más aplicaciones o servicios de la red perimetral simplemente iniciando sesión una vez. Para obtener más información, consulte [Web SSO Design](Web-SSO-Design.md).  
  
    Por ejemplo, puede que Fabrikam quiera que sus clientes tengan único\-sesión\-en \(SSO\) acceso a varias aplicaciones o servicios que se hospedan en su red perimetral.  
  
Los componentes siguientes son necesarios para este objetivo de implementación:  
  
-   **Servicios de dominio de Active Directory \(AD DS\):** El servidor de federación del asociado de recurso debe unirse a un dominio de Active Directory.  
  
-   **DNS perimetral:** Sistema de nombres de dominio \(DNS\) debe contener un host simple \(A\) del registro de recursos para que los equipos cliente puedan encontrar el servidor de federación del asociado de recurso y el servidor Web. El servidor DNS puede hospedar otros registros DNS que también son necesarios en la red perimetral. Para obtener más información, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de federación del asociado de recurso:** El servidor de federación del asociado de recurso valida los tokens de AD FS que envían los asociados de cuenta. Detección de asociado de cuenta se realiza a través de este servidor de federación. Para obtener más información, consulte [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Servidor web:** El servidor web puede hospedar una aplicación web o un servicio web. El servidor web confirma que recibe los tokens de AD FS válidos de usuarios federados antes de permitir el acceso a la aplicación o al servicio web protegido.  
  
    Mediante el uso de Windows Identity Foundation \(WIF\), puede desarrollar su aplicación Web o federada de servicio para que acepte solicitudes de inicio de sesión de usuario que se realizan con cualquier método de inicio de sesión estándar, como nombre de usuario y contraseña.  
  
Después de revisar la información de los temas vinculados, puede empezar a implementar este objetivo siguiendo los pasos descritos en [lista de comprobación: Implementar un diseño de SSO Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) y [lista de comprobación: Implementar un diseño SSO Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
La siguiente ilustración muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.  
  
![el acceso a sus notificaciones](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
