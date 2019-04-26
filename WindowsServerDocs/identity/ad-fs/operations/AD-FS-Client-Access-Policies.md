---
ms.assetid: ''
title: Directivas de Control de acceso de cliente de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc91cd9a446c8ca30471b65374ca99a7bd49d369
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882376"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controlar el acceso a datos de la organización con servicios de federación de Active Directory

Este documento proporciona información general sobre el control de acceso con AD FS en local, los escenarios híbridos y en la nube.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS y el acceso condicional a los recursos locales 
Desde la introducción de los servicios de federación de Active Directory, las directivas de autorización han estado disponibles para restringir o permitir el acceso a los usuarios a los recursos basados en atributos de la solicitud y el recurso.  Como AD FS se haya movido de una versión a otra, cómo se implementan estas directivas ha cambiado.  Para obtener información detallada sobre las características de control de acceso por versión, consulte:
- [Directivas de Control de acceso en AD FS en Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Control de acceso en AD FS en Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS y el acceso condicional en una organización híbrida  

AD FS proporciona el componente de entorno local en de la directiva de acceso condicional en un escenario híbrido. Las reglas de autorización de AD FS en función deben usarse para los recursos que no sean Azure AD, como en el entorno local aplicaciones federadas directamente con AD FS.  El componente en la nube proporcionado por [acceso condicional de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access).  Azure AD Connect proporciona el plano de control que conecta los dos.

Por ejemplo, al registrar los dispositivos con Azure AD para el acceso condicional a los recursos en la nube, el dispositivo de Azure AD Connect reescribir funcionalidad hace que información de registro del dispositivo esté disponible en el entorno local para consumir y aplicar las directivas de AD FS.  De este modo, tiene un enfoque coherente para tener acceso a las directivas de control para ambos en el entorno local y los recursos de nube.  

![Acceso condicional](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>La evolución de las directivas de acceso de cliente para Office 365
Muchos de ustedes usan las directivas de acceso de cliente con AD FS para limitar el acceso a Office 365 y otros servicios de Microsoft Online basándose en factores como la ubicación del cliente y el tipo de aplicación de cliente que se va a usar.  
- [Directivas de acceso de cliente en Windows Server 2012 R2 AD FS](Access-Control-Policies-W2K12.md)
- [Directivas de acceso de cliente de AD FS 2.0](Access-Control-Policies-in-AD-FS-2.md)

Algunos ejemplos de estas directivas incluyen:
- Bloquear todo el acceso de cliente de extranet a Office 365
- Bloquear todo el acceso de cliente de extranet a Office 365, excepto para los dispositivos tienen acceso a Exchange Online para Exchange Active Sync

A menudo es la necesidad de subyacente detrás de estas directivas mitigar el riesgo de pérdida de datos asegurándose de que solo los clientes autorizados, las aplicaciones que no almacenan en caché los datos, o los dispositivos que se pueden deshabilitar de forma remota pueden obtener acceso a los recursos.

Mientras que las directivas anteriores documentadas para AD FS funcionan en los escenarios documentados, tienen limitaciones porque dependen de los datos de cliente que no están disponibles de forma coherente.  Por ejemplo, la identidad de la aplicación cliente solo está disponible para servicios de Exchange Online en función y no para los recursos, como SharePoint Online, donde pueden tener acceso a los mismos datos a través del explorador o un cliente grueso, como Word o Excel.  También AD FS no es consciente de los recursos dentro de Office 365 tiene acceso, como Exchange Online o SharePoint Online.

Para abordar estas limitaciones y proporcionan una manera más eficaz utilizar directivas para administrar el acceso a datos profesionales en Office 365 u otros recursos de Azure AD en función, Microsoft ha introducido el acceso condicional de Azure AD.  Directivas de acceso condicional de AD Azure pueden configurarse para un recurso específico o para cualquier o todos los recursos dentro de Office 365, SaaS o aplicaciones personalizadas en Azure AD.  Estas directivas dinamizar una confianza del dispositivo, ubicación y otros factores.

Para obtener más información sobre el acceso condicional de Azure AD, consulte [acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

Un cambio de clave habilitar estos escenarios es [autenticación moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), una nueva forma de autenticación de usuarios y dispositivos que funciona la misma manera a través de los clientes de Office, Skype, Outlook y exploradores.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre cómo controlar el acceso a través de la nube y locales, consulte:

- [Acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [Directivas de Control de acceso en AD FS 2016](Access-Control-Policies-in-AD-FS.md)
