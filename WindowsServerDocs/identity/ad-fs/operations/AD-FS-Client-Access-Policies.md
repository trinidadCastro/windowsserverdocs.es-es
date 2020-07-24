---
title: Directivas de Access Control de cliente en AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6d049175c7d89670f82bb45addc929d57b60b7b0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960747"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controlar el acceso a los datos de la organización con Servicios de federación de Active Directory (AD FS)

En este documento se proporciona información general sobre el control de acceso con AD FS en escenarios locales, híbridos y en la nube.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS y el acceso condicional a los recursos locales 
Desde la introducción de Servicios de federación de Active Directory (AD FS), las directivas de autorización están disponibles para restringir o permitir el acceso de los usuarios a los recursos basándose en los atributos de la solicitud y el recurso.  Como AD FS ha pasado de la versión a la versión, la manera en que se implementan estas directivas ha cambiado.  Para obtener información detallada sobre las características de control de acceso por versión, vea:
- [Directivas de Access Control en AD FS en Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Control de acceso en AD FS en Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS y acceso condicional en una organización híbrida  

AD FS proporciona el componente local de la Directiva de acceso condicional en un escenario híbrido. Las reglas de autorización basadas en AD FS deben usarse para recursos que no son de Azure AD, como aplicaciones locales federadas directamente en AD FS.  [Azure ad el acceso condicional](/azure/active-directory/active-directory-conditional-access)proporciona el componente en la nube.  Azure AD Connect proporciona el plano de control que conecta los dos.

Por ejemplo, al registrar dispositivos con Azure AD para el acceso condicional a los recursos en la nube, la funcionalidad de reescritura de dispositivos Azure AD Connect hace que la información de registro de dispositivos esté disponible en el entorno local para que las directivas de AD FS utilicen y apliquen.  De este modo, tiene un enfoque coherente para las directivas de control de acceso para los recursos locales y en la nube.  

![acceso condicional](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>La evolución de las directivas de acceso de cliente para Office 365
Muchos de ellos utilizan directivas de acceso de cliente con AD FS para limitar el acceso a Office 365 y a otros servicios en línea de Microsoft basados en factores como la ubicación del cliente y el tipo de aplicación cliente que se está usando.  
- [Directivas de acceso de cliente en Windows Server 2012 R2 AD FS](Access-Control-Policies-W2K12.md)
- [Directivas de acceso de cliente en AD FS 2,0](Access-Control-Policies-in-AD-FS-2.md)

Algunos ejemplos de estas directivas incluyen:
- Bloquear el acceso de todos los clientes de extranet a Office 365
- Bloquear todo el acceso del cliente de extranet a Office 365, excepto para los dispositivos que acceden a Exchange Online para Exchange Active Sync

A menudo, la necesidad subyacente de estas directivas es mitigar el riesgo de pérdida de datos al garantizar que solo los clientes autorizados, las aplicaciones que no almacenan en caché los datos o los dispositivos que se pueden deshabilitar de forma remota pueden obtener acceso a los recursos.

Aunque las directivas documentadas anteriores para AD FS funcionan en los escenarios específicos documentados, tienen limitaciones porque dependen de datos de cliente que no están disponibles de forma coherente.  Por ejemplo, la identidad de la aplicación cliente solo está disponible para los servicios basados en Exchange Online y no para recursos como SharePoint Online, donde se puede tener acceso a los mismos datos a través del explorador o un "cliente grueso" como Word o Excel.  Además AD FS no es consciente del recurso dentro de Office 365 al que se tiene acceso, como SharePoint Online o Exchange Online.

Para abordar estas limitaciones y proporcionar una forma más sólida de usar directivas para administrar el acceso a los datos empresariales en Office 365 u otros recursos basados en Azure AD, Microsoft ha introducido Azure AD el acceso condicional.  Azure AD se pueden configurar directivas de acceso condicional para un recurso específico o para cualquiera de los recursos de Office 365, SaaS o aplicaciones personalizadas de Azure AD.  Estas directivas dinamizan la confianza de los dispositivos, la ubicación y otros factores.

Para obtener más información sobre Azure AD el acceso condicional, consulte [acceso condicional en Azure Active Directory](/azure/active-directory/active-directory-conditional-access)

Un cambio de clave que habilita estos escenarios es la [autenticación moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), una nueva forma de autenticar a los usuarios y dispositivos que funcionan de la misma manera entre los clientes de Office, Skype, Outlook y exploradores.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre cómo controlar el acceso a través de la nube y de forma local, vea:

- [Acceso condicional en Azure Active Directory](/azure/active-directory/active-directory-conditional-access)
- [Access Control directivas en AD FS 2016](Access-Control-Policies-in-AD-FS.md)
