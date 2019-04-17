---
ms.assetid: 
title: Directivas de Control de acceso de cliente de AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5e1e1b907e6fccbf2b9906106d3360bd9a6fd69d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controlar el acceso a datos de la organización con servicios de federación de Active Directory

Este documento proporciona información general de control de acceso con AD FS a través de manera local, híbrido y nube escenarios.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS y acceso condicional a los recursos locales 
Desde la introducción de los servicios de federación de Active Directory, las directivas de autorización han estado disponibles para restringir o permitir a los usuarios acceso a los recursos en función de los atributos de la solicitud y el recurso.  Cómo se implementan estas directivas que como AD FS según la versión se ha movido, ha cambiado.  Para obtener información detallada sobre las características de control de acceso por versión de consulta:
- [Directivas de Control de acceso de AD FS en Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Control de acceso de AD FS en Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS y el acceso condicional de una organización híbrido  

AD FS proporciona el componente de local en de la directiva de acceso condicional en un escenario de híbrido. Las reglas de autorización de AD FS en función deben usarse para los recursos no Azure AD, como en instalaciones federación aplicaciones directamente a AD FS.  Proporciona el componente de nube [acceso condicional de Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access).  Azure AD Connect proporciona el plano de control conectar los dos.

Por ejemplo, cuando te registras dispositivos con Azure AD para acceso condicional a recursos de nube, el dispositivo de Azure AD Connect escribir atrás funcionalidad hace que sea información de registro de dispositivo disponible en locales para consumir y aplicar las directivas de AD FS.  De esta forma, tienes un enfoque coherente para acceder a las directivas de control para ambos en locales y en la nube.  

![Acceso condicional](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>La evolución de las directivas de acceso de cliente de Office 365
Muchos de vosotros usan directivas de acceso de cliente con AD FS para limitar el acceso a Office 365 y otros servicios de Microsoft Online en función de factores como la ubicación del cliente y el tipo de aplicación de cliente se usa.  
- [Directivas de acceso de cliente de Windows Server 2012 R2 AD FS](Access-Control-Policies-W2K12.md)
- [Directivas de acceso de cliente de AD FS 2.0](Access-Control-Policies-in-AD-FS-2.md)

Algunos ejemplos de estas directivas:
- Bloquear todo el acceso de cliente de extranet a Office 365
- Bloquear todo el acceso de cliente de extranet a Office 365, excepto para los dispositivos que tengan acceso a Exchange Online de Exchange Active Sync

A menudo es la necesidad subyacente detrás de estas directivas mitigar el riesgo de pérdida de datos asegurándose solo los clientes autorizados, las aplicaciones que no almacene en caché los datos, o dispositivos que se puede deshabilitar de forma remota pueden obtener acceso a los recursos.

Aunque las directivas documentadas anteriores de AD FS funcionan en los escenarios específicos documentados, tienen limitaciones porque dependen de los datos de cliente que no está disponibles de forma coherente.  Por ejemplo, la identidad de la aplicación cliente solo está disponible para los servicios de Exchange Online según y no para recursos como SharePoint Online, donde pueden tener acceso a los mismos datos a través de un cliente grueso como Word o Excel o el explorador.  También AD FS no conoce el recurso en Office 365 se tiene acceso, como Exchange Online o SharePoint Online.

Para solucionar estas limitaciones y proporcionar una forma más eficaz usar directivas para administrar el acceso a datos empresariales en Office 365 u otros recursos en función de Azure AD, Microsoft ha introducido acceso condicional de Azure AD.  Las directivas de acceso condicional de AD Azure pueden configurarse para un recurso específico o para cualquier o todos los recursos dentro de Office 365, SaaS o aplicaciones personalizadas en Azure AD.  Estas directivas de control dinámico en la confianza del dispositivo, la ubicación y otros factores.

Para obtener más información acerca del acceso condicional de Azure AD, consulta [acceso condicional de Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)

Es un cambio de clave habilitar estos escenarios [autenticación moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), una nueva forma de autenticación de usuarios y dispositivos que funciona del mismo modo en los clientes de Office, Skype, Outlook y exploradores.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre cómo controlar el acceso a través de la nube y local, consulta:

- [Acceso condicional de Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)
- [Directivas de Control de acceso de AD FS de 2016](Access-Control-Policies-in-AD-FS.md)
