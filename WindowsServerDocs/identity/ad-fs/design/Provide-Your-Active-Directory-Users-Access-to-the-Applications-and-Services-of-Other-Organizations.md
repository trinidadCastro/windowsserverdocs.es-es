---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios de otras organizaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fc0cbb461a48d04ddaa677d4de2369ef58fd5390
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190961"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios de otras organizaciones

Este Active Directory Federation Services \(AD FS\) objetivo de implementación se basa en el objetivo de [proporcionar Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Si es administrador en la organización del asociado de cuenta y tiene un objetivo de implementación para proporcionar acceso federado para los empleados a los recursos hospedados de otra organización:  
  
-   Los empleados que iniciaron sesión a un dominio de Active Directory en la red corporativa pueden usar solo\-sesión\-en \(SSO\) funcionalidad para tener acceso a varias Web\-en función de las aplicaciones o servicios, que están protegidas por AD FS, cuando las aplicaciones o servicios están en una organización diferente. Para obtener más información, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Por ejemplo, es posible que Fabrikam desee que sus empleados de la red corporativa tengan acceso federado a servicios web que se hospedan en Contoso.  
  
-   Los empleados remotos que iniciaron sesión a un dominio de Active Directory pueden obtener tokens de AD FS desde el servidor de federación de su organización para obtener acceso federado a AD FS protegida Web\-en función de las aplicaciones o servicios que se hospedan en otra organización.  
  
    Por ejemplo, es posible que Fabrikam desee que sus empleados remotos tengan acceso federado a servicios de AD FS protegida que se hospedan en Contoso, sin necesidad de que los empleados de Fabrikam estén en la red corporativa de Fabrikam.  
  
Además de los componentes básicos que se describen en [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) y que aparecen sombreados en la siguiente ilustración, los siguientes componentes son necesarios para este objetivo de implementación:  
  
-   **Proxy de servidor de federación de cuentas asociado:** Los empleados que tienen acceso el servicio o aplicación federado desde Internet pueden usar este componente de AD FS para realizar la autenticación. De forma predeterminada, este componente realiza la autenticación de formularios, pero también puede realizar la autenticación básica. También puede configurar este componente para llevar a cabo Secure Sockets Layer \(SSL\) autenticación del cliente si los empleados de su organización tienen certificados para presentar. Para obtener más información, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS perimetral:** Esta implementación de sistema de nombres de dominio \(DNS\) proporciona los nombres de host de la red perimetral. Para obtener más información acerca de cómo configurar DNS perimetral para un servidor proxy de federación, consulte [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Empleado remoto:** El empleado remoto accede a un Web\-aplicación basada en \(a través de un explorador Web compatible\) o una Web\-servicio basado en \(a través de una aplicación\), con credenciales válidas de la red corporativa, mientras está fuera del sitio mediante Internet. Equipo de cliente del empleado en la ubicación remota se comunica directamente con el servidor proxy de federación para generar un token y autenticarse en la aplicación o servicio.  
  
Después de revisar la información de los temas vinculados, puede empezar a implementar este objetivo siguiendo los pasos descritos en [lista de comprobación: Implementar un diseño de SSO Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
La siguiente ilustración muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.  
  
![el acceso a las aplicaciones](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
