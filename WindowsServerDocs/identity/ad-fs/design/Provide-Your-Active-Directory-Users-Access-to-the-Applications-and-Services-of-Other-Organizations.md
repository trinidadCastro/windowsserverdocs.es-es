---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Proporcionar a los usuarios de Active Directory acceso a las aplicaciones y servicios de otras organizaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0d1bf60c276932a77573acff2e4e011831e65a05
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967572"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Proporcionar a los usuarios de Active Directory acceso a las aplicaciones y servicios de otras organizaciones

Esta Servicios de federación de Active Directory (AD FS) \( AD FS \) objetivo de implementación se basa en el objetivo de [proporcionar a los usuarios Active Directory acceso a sus aplicaciones y servicios para notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).

Si es administrador en la organización del asociado de cuenta y tiene un objetivo de implementación para proporcionar acceso federado para los empleados a los recursos hospedados de otra organización:

-   Los empleados que han iniciado sesión en un dominio de Active Directory de la red corporativa pueden usar la \- \- funcionalidad de inicio de sesión único de \( SSO \) para tener acceso a varios \- servicios o aplicaciones basados en Web, que están protegidos por AD FS, cuando las aplicaciones o los servicios están en una organización diferente. Para obtener más información, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).

    Por ejemplo, es posible que Fabrikam desee que sus empleados de la red corporativa tengan acceso federado a servicios web que se hospedan en Contoso.

-   Los empleados remotos que han iniciado sesión en un dominio de Active Directory pueden obtener AD FS tokens del servidor de Federación de su organización para obtener acceso federado a AD FS \- aplicaciones o servicios basados en Web protegidos que se hospedan en otra organización.

    Por ejemplo, es posible que Fabrikam desee que sus empleados remotos tengan acceso federado a AD FS: servicios protegidos que se hospedan en Contoso, sin necesidad de que los empleados de Fabrikam estén en la red corporativa de fabrikam.

Además de los componentes básicos que se describen en [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) y que aparecen sombreados en la siguiente ilustración, los siguientes componentes son necesarios para este objetivo de implementación:

-   **Proxy de servidor de Federación del asociado de cuenta:** Los empleados que tienen acceso al servicio o aplicación federado desde Internet pueden usar este componente de AD FS para realizar la autenticación. De forma predeterminada, este componente realiza la autenticación de formularios, pero también puede realizar la autenticación básica. También puede configurar este componente para realizar Capa de sockets seguros la \( \) autenticación del cliente SSL si los empleados de su organización tienen certificados para presentar. Para más información, consulte [Ubicación de un servidor proxy de federación](Where-to-Place-a-Federation-Server-Proxy.md).

-   **DNS perimetral:** Esta implementación de DNS del sistema de nombres de dominio \( \) proporciona los nombres de host para la red perimetral. Para obtener más información acerca de cómo configurar el DNS perimetral para un servidor proxy de Federación, consulte [requisitos de resolución de nombres para servidores proxy de Federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).

-   **Empleado remoto:** El empleado remoto obtiene acceso a una \- aplicación basada en Web \( a través de un explorador Web compatible \) o un \- servicio basado en Web \( a través de una aplicación \) , usando credenciales válidas de la red corporativa, mientras el empleado está fuera del sitio a través de Internet. El equipo cliente del empleado en la ubicación remota se comunica directamente con el servidor proxy de Federación para generar un token y autenticarse en la aplicación o el servicio.

Después de revisar la información de los temas vinculados, puedes empezar a implementar este objetivo siguiendo los pasos descritos en [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).

En la siguiente ilustración se muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.

![acceder a las aplicaciones](media/50af4837-31e0-451f-a942-e705c2300065.gif)

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
