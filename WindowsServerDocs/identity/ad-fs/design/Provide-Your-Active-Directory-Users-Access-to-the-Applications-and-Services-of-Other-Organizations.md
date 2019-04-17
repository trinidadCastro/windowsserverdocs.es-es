---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Proporcionar el acceso a los usuarios de Active Directory para las aplicaciones y servicios de otras organizaciones
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d50d26c5c654e5c91b82f6f209e21f257221c12d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Proporcionar el acceso a los usuarios de Active Directory para las aplicaciones y servicios de otras organizaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este objetivo de implementación de servicios de federación de Active Directory \(AD FS\) se basa en el objetivo de [proporcionar tu Active Directory a los usuarios acceso a tus aplicaciones para notificaciones y los servicios](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Cuando es un administrador de la organización de partner de cuenta y tiene un objetivo de implementación para proporcionar acceso federado para los empleados hospedado recursos en otra organización:  
  
-   Los empleados que están conectados a un dominio de Active Directory en la red corporativa pueden usar single\ sign\ en \(SSO\) funcionalidad para acceder a varias aplicaciones basadas en Web\ o servicios, que están protegidos por AD FS, cuando las aplicaciones o servicios que se encuentren en otra organización. Para obtener más información, consulta [federados Web SSO diseño](Federated-Web-SSO-Design.md).  
  
    Por ejemplo, Fabrikam puede los empleados de la red corporativa tengan acceso a servicios Web que se hospedan en Contoso federado.  
  
-   Los empleados remotos que han iniciado sesión a un dominio de Active Directory pueden obtener tokens de AD FS desde el servidor de federación de la organización para obtener acceso federado a AD FS seguro Web\ aplicaciones o servicios que están hospedados en otra organización.  
  
    Por ejemplo, Fabrikam puede sus empleados tengan acceso a los servicios de AD FS seguro que se hospedan en Contoso, sin necesidad de que los empleados de Fabrikam esté en la red corporativa de Fabrikam federado remotos.  
  
Además de los componentes básicos que se describen en [proporcionar tu Active Directory a los usuarios acceso a tus aplicaciones para notificaciones y los servicios](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) y que se sombreado en la siguiente ilustración, los siguientes componentes son necesarios para este objetivo de implementación:  
  
-   **Cuenta de proxy del servidor de federación de partner:** los empleados que tienen acceso a federados servicio o aplicación de Internet pueden usar este componente AD FS para realizar la autenticación. De manera predeterminada, este componente realiza la autenticación de formularios, pero también puede realizar la autenticación básica. También puedes configurar este componente para realizar la autenticación de cliente de capa de Sockets seguros \(SSL\) si los empleados de la organización tienen certificados para presentar. Para obtener más información, consulta [dónde colocar un Proxy de servidor de federación](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **Perimetral DNS:** esta implementación de sistema de nombres de dominio \(DNS\) proporciona los nombres de host de la red de perímetro. Para obtener más información sobre cómo configurar perímetro DNS para un proxy de federación de servidor, consulta [requisitos de resolución de nombre de Proxies de servidor de federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Los empleados remotos:** el empleado remoto accede a una aplicación basada en Web\ \ (a través de un browser\ Web compatible) o un servicio basado en Web\ \ (a través de un application\), con las credenciales válidas de la red corporativa, mientras que el empleado es fuera del sitio con Internet. El equipo del empleado cliente en la ubicación remota se comunica directamente con el proxy de servidor de federación para generar un token y autenticarse en la aplicación o servicio.  
  
Después de revisar la información de los temas vinculados, puedes empezar a implementar este objetivo siguiendo los pasos de [lista de comprobación: implementar un diseño de inicio de sesión único Web federados](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
La siguiente ilustración muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.  
  
![Acceso a tus aplicaciones](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
