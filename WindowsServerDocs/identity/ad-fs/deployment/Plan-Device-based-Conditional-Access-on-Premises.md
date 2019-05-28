---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Planeación del acceso condicional basado en dispositivos a nivel local
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ffd7131f7f3772ab47b62c9755008fe3b1c4b274
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192071"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planeación del acceso condicional basado en dispositivos a nivel local


Este documento describe las directivas de acceso condicional basadas en dispositivos en un escenario híbrido que se conectan los directorios locales a Azure AD con Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>Acceso condicional de AD FS y híbridas  

AD FS proporciona el componente de entorno local en directivas de acceso condicional de un escenario híbrido.  Al registrar los dispositivos con Azure AD para el acceso condicional a los recursos en la nube, el dispositivo de Azure AD Connect escritura diferida de funcionalidad hace que información de registro del dispositivo esté disponible en el entorno local para consumir y aplicar las directivas de AD FS.  De este modo, tiene un enfoque coherente para tener acceso a las directivas de control para ambos en el entorno local y los recursos de nube.  

![Acceso condicional](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipos de dispositivos registrados  
Hay tres tipos de dispositivos registrados, todos los cuales se representan como objetos de dispositivo en Azure AD y se pueden usar para el acceso condicional con AD FS como local.  

| |Agregar trabajo o educativa  |Unión a Azure AD  |Combinación Domian Windows 10    
| --- | --- |--- | --- |
|Descripción    |  Los usuarios agregar su trabajo o educativa a su dispositivo BYOD interactivamente.  **Nota:** Agregar cuenta profesional o educativa es el reemplazo de Workplace Join en Windows 8/8.1       | Los usuarios inscribir sus dispositivos de trabajo de Windows 10 en Azure AD.|Dispositivos de Windows 10 Unidos a un dominio se registrarán automáticamente con Azure AD.|           
|Cómo los usuarios iniciar sesión en el dispositivo     |  Sin inicio de sesión de Windows como la cuenta profesional o educativa.  Inicio de sesión con una cuenta Microsoft.       |   Inicie sesión en Windows como la cuenta (profesional o educativa) que registra el dispositivo.      |     Inicio de sesión con cuenta de AD.|      
|¿Cómo se administran los dispositivos    |      Directivas de MDM (con la inscripción de Intune adicional)   | Directivas de MDM (con la inscripción de Intune adicional)        |   Directiva de grupo, System Center Configuration Manager (SCCM) |
|Tipo de confianza de AD de Azure|Espacio de trabajo unido|Unidos a Azure AD|Pertenencia a un dominio  |     
|Ubicación de la configuración con 10    | Configuración > cuentas > su cuenta > Agregar una cuenta profesional o educativa        | Configuración > sistema > acerca de > unirse a Azure AD       |   Configuración > sistema > acerca de > unirse a un dominio |       
|¿También está disponible para dispositivos iOS y Android?   |    Sí     |       No  |   No   |   

  

Para obtener más información sobre las diferentes maneras para registrar dispositivos, vea también:  
* [Uso de dispositivos de Windows 10 en el área de trabajo](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configuración de dispositivos Windows 10 para el trabajo](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Únase a Windows 10 Mobile a Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Cómo Windows 10 usuario y dispositivo de inicio de sesión es diferente de las versiones anteriores  
Para Windows 10 y AD FS 2016, hay algunos aspectos de autenticación y registro de dispositivo nuevo que debe conocer (especialmente si está muy familiarizado con el registro de dispositivos y "unión" en versiones anteriores).  

En primer lugar, en Windows 10 y AD FS en Windows Server 2016, autenticación y registro de dispositivo ya no se basan únicamente en un X509 certificado de usuario.  Hay un protocolo nuevo y más eficaz que proporciona una mejor seguridad y una experiencia de usuario más sencilla.  Las principales diferencias son que, para la unión de dominios de Windows 10 y Azure AD Join, hay un X509 certificado de equipo y una nueva credencial denominan una PRT.  Puede leer toda la información [aquí](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) y [aquí](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

En segundo lugar, Windows 10 y AD FS 2016 admiten la autenticación de usuario con Microsoft Passport for Work, que puede leer sobre [aquí](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) y [aquí](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 proporciona SSO basado en PRT y Passport credenciales de usuario y dispositivo sin problemas.  Siguiendo los pasos de este documento, puede habilitar estas funcionalidades y verlos en el trabajo.  

### <a name="device-access-control-policies"></a>Directivas de Control de acceso de dispositivo  
Como se pueden usar dispositivos de reglas de control de acceso de AD FS sencillas:  

- permitir el acceso solo desde un dispositivo registrado   
- requerir autenticación multifactor cuando un dispositivo no está registrado  

Estas reglas, a continuación, se pueden combinar con otros factores como la ubicación de acceso de red y autenticación multifactor, creación de directivas de acceso condicional enriquecido, como:  


- requerir autenticación multifactor para obtener acceso desde fuera de la red corporativa, excepto para los miembros de un grupo determinado o grupos de dispositivos no registrados  

Con AD FS 2016, se pueden configurar estas directivas específicamente para requieren un nivel de confianza de dispositivo en particular: cualquier **autenticado**, **administrada**, o **conforme**.  

Para obtener más información sobre la configuración de AD FS directivas de control de acceso, consulte [las directivas de control de acceso en AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivos autenticados  
Dispositivos autenticados son dispositivos registrados que no están inscritos en MDM (Intune y 3ª parte MDMs para Windows 10, Intune solo para iOS y Android).   

Dispositivos autenticados tendrán el **isManaged** de notificaciones de AD FS con valor **FALSE**. (Mientras que los dispositivos que no están registrados en absoluto carecerá de esta notificación.)  Dispositivos autenticados (y todos los dispositivos registrados) tendrá el isKnown AD FS una notificación con un valor **TRUE**.  

#### <a name="managed-devices"></a>Dispositivos administrados:   

Los dispositivos administrados son dispositivos registrados que están inscritos con MDM.  

Los dispositivos administrados tendrá el isManaged AD FS una notificación con un valor **TRUE**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivos compatibles (con MDM o las directivas de grupo)  
Dispositivos compatibles son los dispositivos registrados que no están inscritos solo con MDM, pero cumplir con las directivas MDM. (Información de cumplimiento se origina con el MDM y se escribe en Azure AD).  

Dispositivos compatibles tendrán el **isCompliant** de notificaciones de AD FS con valor **TRUE**.    

Para obtener una lista completa de dispositivos de AD FS 2016 y notificaciones de acceso condicional, consulte [referencia](#reference).  


## <a name="reference"></a>Referencia  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Lista completa de nuevas notificaciones de AD FS 2016 y dispositivo  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/displayname  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/identifier  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ostype  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/osversion  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser  
* https://schemas.microsoft.com/2014/02/devicecontext/claims/isknown  
* https://schemas.microsoft.com/2014/02/deviceusagetime  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/trusttype  
* https://schemas.microsoft.com/claims/authnmethodsreferences  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path  
* https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/client-request-id  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/relyingpartytrustid  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-ip  
* https://schemas.microsoft.com/2014/09/requestcontext/claims/userip  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod  
