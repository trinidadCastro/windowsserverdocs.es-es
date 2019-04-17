---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Planear el acceso condicional basadas en el dispositivo local
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6303bba92ce4f4f99ae8e5095060b06885e427d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planear el acceso condicional basadas en el dispositivo local

>Se aplica a: Windows Server 2016

Este documento describen las directivas de acceso condicional basadas en dispositivos en un escenario híbrido donde los directorios locales están conectados a Azure AD con Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS y híbrido acceso condicional  

AD FS proporciona el componente de local en de directivas de acceso condicional en un escenario de híbrido.  Cuando te registras dispositivos con Azure AD para acceso condicional a recursos de nube, el dispositivo de Azure AD Connect reescribir funcionalidad hace que sea información de registro de dispositivo disponible en locales para consumir y aplicar las directivas de AD FS.  De esta forma, tienes un enfoque coherente para acceder a las directivas de control para ambos en locales y en la nube.  

![Acceso condicional](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipos de dispositivos registrados  
Existen tres tipos de dispositivos registrados, que se representan como objetos de dispositivo en Azure AD y puede usarse para acceso condicional con AD FS local, así como.  

| |Agregar el trabajo o colegio cuenta  |Unión a Azure AD  |Combinación de Domian de Windows 10    
| --- | --- |--- | --- |
|Descripción    |  Los usuarios agregar su trabajo o centro docente cuenta para sus dispositivos BYOD de forma interactiva.  **Nota:** Agregar cuenta profesional o educativa es la sustitución de al área de trabajo en Windows 8 y 8.1       | Los usuarios unir el dispositivo de trabajo de Windows 10 a Azure AD.|Dispositivos Unidos a un dominio de Windows 10 automáticamente registra con Azure AD.|           
|Cómo los usuarios iniciar sesión en el dispositivo     |  No inicia sesión en Windows como la cuenta profesional o educativa.  Inicia sesión con una cuenta de Microsoft.       |   Inicia sesión en Windows como la cuenta (profesional o educativa) que registra el dispositivo.      |     Inicia sesión con la cuenta de AD.|      
|¿Cómo se administran dispositivos    |      Directivas de MDM (con la inscripción de Intune adicional)   | Directivas de MDM (con la inscripción de Intune adicional)        |   Directiva de grupo, System Center Configuration Manager (SCCM) |
|Tipo de confianza de AD Azure|Área de trabajo unido|Azure AD unido|Unido a un dominio  |     
|Ubicación de la configuración de W10    | Configuración > cuentas > tu cuenta > Agregar una cuenta profesional o educativa        | Configuración > sistema > acerca de > unirte a Azure AD       |   Configuración > sistema > acerca de > unirse a un dominio |       
|¿También disponible para dispositivos iOS y Android?   |    Sí     |       No  |   No   |   

  

Para obtener más información sobre las diferentes formas para registrar los dispositivos, consulta también:  
* [Uso de dispositivos de Windows 10 en tu empresa](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configuración de dispositivos de Windows 10 para el trabajo](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Unir Windows 10 Mobile a Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Cómo los usuarios de Windows 10 y dispositivo de inicio de sesión es diferente de versiones anteriores  
Para Windows 10 y hay algunos nuevos aspectos de registro del dispositivo y la autenticación de AD FS 2016 debes conocer (especialmente si estás familiarizado con el registro del dispositivo y "al área de trabajo" en versiones anteriores).  

En primer lugar, en Windows 10 y AD FS en Windows Server 2016, autenticación y el registro del dispositivo ya no reside únicamente en un X509 certificado de usuario.  Hay un protocolo nuevo y más sólido que proporciona mayor seguridad y una experiencia de usuario más sencilla.  Las principales diferencias son que, para la unión de dominios de Windows 10 y unión a Azure AD, hay un X509 certificado de equipo y una nueva credencial llama a un Imp.  Puedes leer todo sobre ella [aquí](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) y [aquí](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

En segundo lugar, Windows 10 y AD FS 2016 admiten la autenticación de usuario con Microsoft Passport for Work, que puede leer sobre [aquí](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) y [aquí](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 proporciona un dispositivo sin problemas y SSO basado en Imp y Passport credenciales de usuario.  Siguiendo los pasos de este documento, puedes habilitar estas funcionalidades y verlos a trabajar.  

### <a name="device-access-control-policies"></a>Directivas de Control de acceso de dispositivo  
Dispositivos pueden usarse en las reglas de control de acceso de AD FS simples, como:  

- permitir el acceso solo desde un dispositivo registrado   
- requerir la autenticación de factor de multi cuando un dispositivo no está registrado  

Estas reglas, a continuación, se pueden combinar con otros factores como la ubicación de acceso de red y autenticación de factor de multi, la creación de directivas de acceso condicional enriquecido, como:  


- requerir la autenticación de factor de multi para dispositivos sin registrar acceso desde fuera de la red corporativa, excepto para los miembros de un determinado grupo o grupos  

Con AD FS de 2016, estas directivas pueden configurarse específicamente para requerir un nivel de confianza de dispositivo en particular: cualquier **autenticado**, **administrado**, o **compatible con**.  

Para obtener más información sobre cómo configurar AD FS las directivas de control de acceso, consulta [directivas de control de acceso de AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivos autenticados  
Dispositivos autenticados son los dispositivos registrados que no están inscritos en MDM (Intune y terceros 3 MDM para Windows 10, Intune solo para iOS y Android).   

Dispositivos autenticados tendrán la **isManaged** AD FS reclamar con valor **FALSE**. (Mientras que los dispositivos que no estén registrados en absoluto carecerá de esta notificación.) Dispositivos autenticados (y todos los dispositivos) tendrá la isKnown AD FS reclamar con valor **TRUE**.  

#### <a name="managed-devices"></a>Dispositivos administrados:   

Dispositivos administrados son los dispositivos registrados inscritos con MDM.  

Dispositivos administrados tendrá la isManaged AD FS reclamar con valor **TRUE**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivos compatibles (con directivas de grupo o MDM)  
Compatible con dispositivos son los dispositivos registrados que no están inscritos solo con MDM pero cumple con las directivas MDM. (Información de cumplimiento se origina con MDM y se escribe en Azure AD).  

Dispositivos compatibles con tendrá la **isCompliant** AD FS reclamar con valor **TRUE**.    

Para una lista completa de dispositivo de AD FS 2016 y reclamaciones de acceso condicional, consulta [referencia](#reference).  


## <a name="reference"></a>Referencia  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Lista completa de nuevo reclamaciones de 2016 FS y dispositivos de anuncios  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/UPN  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Name  
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
