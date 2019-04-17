---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novedades en Active Directory federación servicios para Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8dd9174a869110d98ba1e65cbf2c2b6ec000b71
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2018
---
# <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novedades en Active Directory federación servicios para Windows Server 2016

>Se aplica a: Windows Server 2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novedades en Active Directory federación servicios para Windows Server 2016   
Si buscas información sobre las versiones anteriores de AD FS, consulte los siguientes artículos:  
 [ADFS en Windows Server 2012 o 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) y [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Los servicios de federación de Active Directory proporciona el control de acceso y el inicio de sesión único en una amplia variedad de aplicaciones de Office 365, nube en función de las aplicaciones SaaS y aplicaciones de la red corporativa.  
* Para la organización de TI, que permite proporcionar inicio de sesión en y control de acceso a las aplicaciones modernas y heredadas, local y en la nube, basado en el mismo conjunto de directivas y las credenciales.    
* Para el usuario, proporciona un inicio de sesión transparente sobre el uso de las credenciales de cuenta mismo, te resultará familiar.  
* Para el desarrollador, proporciona una manera sencilla de autenticar a los usuarios cuyas identidades live en el directorio de la organización para que se puede Centrar tus esfuerzos en tu aplicación, no autenticación o identidad.  

Este artículo describe lo que es nuevo en AD FS en Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Eliminar las contraseñas de la Extranet  
AD FS 2016 permite tres nuevas opciones de inicio de sesión sin contraseñas, permitiendo que las organizaciones evitar el riesgo de red poner en peligro de phished, las contraseñas perdidas o robadas. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Inicia sesión con Azure la autenticación multifactor
AD FS 2016 basa en la autenticación multifactor capacidades (AMF) de AD FS en Windows Server 2012 R2 al permitir el inicio de sesión sobre el uso de un código de MFA de Azure, sin especificar primero un nombre de usuario y una contraseña.

* Con Azure AMF como método de autenticación principal, se pide al usuario su nombre de usuario y el código OTP desde la aplicación Azure Authenticator.  
* Con Azure AMF como método de autenticación secundario o adicional, el usuario proporciona autenticación principal credenciales (mediante la autenticación integrada en Windows, nombre de usuario y contraseña, tarjetas inteligentes o certificados de usuario o dispositivo) y, luego, ve un mensaje de texto, voz, o OTP en función de inicio de sesión de MFA de Azure.  
* Con el nuevo adaptador de Azure MFA integrado, instalación y configuración de MFA de Azure con AD FS nunca ha sido más sencillo.
* Las organizaciones pueden sacar provecho de Azure MFA sin necesidad de un servidor de MFA de Azure de local.
* Azure MFA puede configurarse para intranet o extranet o como parte de cualquier directiva de control de acceso.

Para obtener más información sobre Azure MFA con AD FS
*  [Configurar AD FS 2016 y MFA de Azure](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Acceso sin contraseña desde dispositivos compatibles con
AD FS 2016 se basa en el anterior capacidades de registro de dispositivo para habilitar el inicio de sesión en y el control de acceso según el estado de cumplimiento del dispositivo. Los usuarios pueden iniciar sesión sobre el uso de las credenciales de dispositivo y cumplimiento se vuelve a evaluar cuando cambien los atributos del dispositivo, para que siempre puede asegurarse de que se aplica directivas.  Esto permite que las directivas como

* Habilitar el acceso solo de los dispositivos que son compatibles con o administrado  
* Habilitar el acceso Extranet solo de los dispositivos que son compatibles con o administrado  
* Requerir la autenticación multifactor para equipos que no administrado o no son compatibles  

AD FS proporciona el componente de local en de directivas de acceso condicional en un escenario de híbrido. Cuando te registras dispositivos con Azure AD para acceso condicional a recursos de nube, la identidad del dispositivo puede usarse para así como las directivas de AD FS.

![Novedades](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Para obtener más información sobre cómo usar el dispositivo en función de acceso condicional en la nube   
 *  [Acceso condicional de Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)

Para obtener más información sobre cómo usar el dispositivo en función de acceso condicional con AD FS
*  [La planeación del dispositivo en función de acceso condicional con AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Directivas de Control de acceso de AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Inicia sesión con Windows Hello para empresas   
Dispositivos de Windows 10 introducen Windows Hello y Windows Hello para empresas, reemplaza las contraseñas de usuario con credenciales de usuario enlazada a dispositivo seguras protegidas por los gestos de un usuario (un PIN, un gesto biométrico, como las huellas digitales o reconocimiento facial). AD FS 2016 admite estos nuevos estas nuevas capacidades de Windows 10 para que los usuarios pueden iniciar sesión en aplicaciones de AD FS de la intranet o en la extranet sin necesidad de proporcionar una contraseña.

Para obtener más información sobre el uso de Microsoft Windows Hello para empresas en la organización
*  [Habilitar Windows Hello para empresas en la organización](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Proteger el acceso a las aplicaciones

### <a name="modern-authentication"></a>Autenticación moderna
AD FS 2016 admite los protocolos modernos más recientes que proporcionan una mejor experiencia de usuario para Windows 10, así como los últimos iOS y dispositivos Android y aplicaciones.  

Para obtener más información, consulta [escenarios de AD FS para desarrolladores](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurar las directivas de control de acceso sin tener que saber reclamar el idioma de reglas  
Anteriormente, los administradores de AD FS tenían que configurar las directivas con el lenguaje de regla de notificación de AD FS, lo que dificulta configurar y mantener directivas. Con las directivas de control de acceso, los administradores pueden usar plantillas para aplicar directivas comunes como
* Permitir el acceso a la intranet solo
* Permitir que todos los usuarios y requieren MFA desde Extranet
* Permitir que todos los usuarios y requieren MFA de un grupo específico

Las plantillas son fáciles de usar a un asistente controlada por el proceso para agregar las reglas de directivas adicionales o de excepciones de personalizar y pueden aplicarse a una o varias aplicaciones de la aplicación de directivas coherente.

Para obtener más información, consulta [directivas de control de acceso de AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Habilitar el inicio de sesión con directorios LDAP que no sean de anuncios  
Muchas organizaciones tienen una combinación de Active Directory y directorios de terceros. Con la adición de compatibilidad de AD FS para autenticar a los usuarios almacenados en directorios compatibles con v3 LDAP, AD FS ahora puede usarse para:
* Usuarios de terceros, directorios LDAP v3 compatibles con
* Usuarios de los bosques de Active Directory para que no está configurada una relación de confianza bidireccional de Active Directory
* Usuarios de Active Directory Rights Management Services (AD LDS)

Para obtener más información, consulta [configurar AD FS para autenticar usuarios almacenados en directorios LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Mejor experiencia de inicio de sesión
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalizar la experiencia para aplicaciones de AD FS de inicio de sesión  
Hemos recibido que la capacidad de personalizar la experiencia de inicio de sesión para cada aplicación sería una mejora excelente de facilidad de uso, especialmente para las organizaciones que proporcionan el inicio de sesión en para las aplicaciones que se representan varias empresas diferentes o marcas.  

Anteriormente, AD FS en Windows Server 2012 R2 proporcionan un inicio de sesión comunes en la experiencia para todas las aplicaciones de terceros de confianza, con la capacidad para personalizar un subconjunto de texto en la función contenido por aplicación. Con Windows Server 2016, puedes personalizar no solo los mensajes, pero imágenes, logotipo y web tema por aplicación. Además, puedes crear nuevos temas personalizados web y aplicarlos por confiar terceros.  

Para obtener más información, consulta [personalización de inicio de sesión del usuario de AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Mejoras operativas y facilidad de uso  
La siguiente sección describe los escenarios de operaciones mejorados que se introducen con servicios de federación de Active Directory en Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Mejorada la auditoría de administración más sencilla  
En AD FS de Windows Server 2012 R2 allí se numerosos eventos de auditoría generados para una sola solicitud y la información relevante acerca de un registro o actividad de emisión de token está ausente (en algunas versiones de AD FS) o que se extienda a través de varios eventos de auditoría. De manera predeterminada la AD FS eventos de auditoría están desactivados debido a su naturaleza detallada.  
Con el lanzamiento de AD FS de 2016, auditoría ha vuelto más fluida y menos detallado.  

Para obtener más información, consulta [auditoría mejoras de AD FS en Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Mejora la interoperabilidad con SAML 2.0 para la participación en confederaciones  
AD FS 2016 contiene compatibilidad de protocolo SAML adicional, incluida la capacidad de importar basado en metadatos que contiene varias entidades de confianza. Esto te permite configurar AD FS para participar en confederaciones como federación InCommon y otras implementaciones de acuerdo con el estándar 2.0 eGov.  

Para obtener más información, consulta [mejora la interoperabilidad con SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Federados de administración de contraseñas simplificada de los usuarios de O365  
Puedes configurar los servicios de federación de Active Directory (AD FS) para enviar notificaciones de expiración de contraseña para las confianzas de terceros de confianza (aplicaciones) que están protegidas por AD FS. ¿Cómo se usan estas notificaciones depende de la aplicación. Por ejemplo, con Office 365, como el usuario de confianza, las actualizaciones se han implementado en Exchange y Outlook para notificar a los usuarios federados de sus contraseñas pronto-a--expirar.  

Para obtener más información, consulta [configurar AD FS para enviar notificaciones de expiración de contraseña.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Cambio de AD FS en Windows Server 2012 R2 a AD FS en Windows Server 2016 es más fácil  
Anteriormente, la migración a una nueva versión de AD FS requería exportar la configuración de la antigua granja e importar a un conjunto de nuevo, en paralelo.  

Ahora, cambio de AD FS en Windows Server 2012 R2 a AD FS en Windows Server 2016 ha convertido en mucho más fácil. Solo tienes que agregar un nuevo servidor de Windows Server 2016 una granja de servidores de Windows Server 2012 R2, y el conjunto de la acción que realizará en el nivel de comportamiento de granja de servidores de Windows Server 2012 R2, por lo que busca y se comporta como un conjunto de Windows Server 2012 R2.  

A continuación, agrega nuevos servidores de Windows Server 2016 al conjunto, compruebe la funcionalidad y quitar los servidores más antiguos de la carga de equilibrado. Una vez que todos los nodos de granja de servidores ejecutan Windows Server 2016, estás listo para actualizar el nivel de comportamiento de granja a 2016 y empezar a usar las nuevas características.  

Para obtener más información, consulta [actualizar a AD FS en Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
