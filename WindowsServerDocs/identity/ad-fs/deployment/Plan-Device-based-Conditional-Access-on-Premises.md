---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Planeación del acceso condicional basado en dispositivos a nivel local
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f3850454f9e2e426ce2d00112adf90f0d2530d8f
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964747"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planeación del acceso condicional basado en dispositivos a nivel local


En este documento se describen las directivas de acceso condicional basadas en dispositivos en un escenario híbrido en el que los directorios locales están conectados a Azure AD mediante Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS y acceso condicional híbrido  

AD FS proporciona el componente local de las directivas de acceso condicional en un escenario híbrido.  Al registrar los dispositivos con Azure AD para el acceso condicional a los recursos en la nube, la funcionalidad de reescritura de dispositivos Azure AD Connect hace que la información de registro de dispositivos esté disponible en el entorno local para que las directivas de AD FS utilicen y apliquen.  De este modo, tiene un enfoque coherente para las directivas de control de acceso para los recursos locales y en la nube.  

![acceso condicional](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipos de dispositivos registrados  
Hay tres tipos de dispositivos registrados, todos ellos se representan como objetos de dispositivo en Azure AD y se pueden usar para el acceso condicional con AD FS también local.  

| |Agregar una cuenta profesional o educativa  |Azure AD Join  |Unión a un dominio de Windows 10    
| --- | --- |--- | --- |
|Descripción    |  Los usuarios agregan su cuenta profesional o educativa a su dispositivo BYOD de forma interactiva.  **Nota:** Agregar una cuenta profesional o educativa es el reemplazo de Workplace Join en Windows 8/8.1       | Los usuarios unen su dispositivo de trabajo de Windows 10 a Azure AD.|Los dispositivos Unidos a un dominio de Windows 10 se registran automáticamente con Azure AD.|           
|Cómo los usuarios inician sesión en el dispositivo     |  No hay inicio de sesión en Windows como cuenta profesional o educativa.  Inicie sesión con un cuenta de Microsoft.       |   Inicie sesión en Windows como cuenta (profesional o educativa) que registró el dispositivo.      |     Inicie sesión con la cuenta de AD.|      
|Cómo se administran los dispositivos    |      Directivas MDM (con inscripción de Intune adicional)   | Directivas MDM (con inscripción de Intune adicional)        |   Directiva de grupo, Configuration Manager |
|Tipo de confianza Azure AD|Unido al área de trabajo|Unido a Azure AD|Pertenencia a un dominio  |     
|Ubicación de configuración de W10    | Configuración > cuentas > la cuenta > agregar una cuenta profesional o educativa        | Configuración > > del sistema acerca de > join Azure AD       |   Configuración > > del sistema acerca de > unirse a un dominio |       
|¿También está disponible para dispositivos iOS y Android?   |    Sí     |       No  |   No   |   

  

Para obtener más información sobre las distintas formas de registrar dispositivos, vea también:  
* [Uso de dispositivos de Windows 10 en el área de trabajo](/azure/active-directory/devices/overview)  
* [Configuración de dispositivos Windows 10 para el trabajo](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Unir Windows 10 Mobile a Azure Active Directory](/windows/client-management/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Cómo el inicio de sesión de usuario y dispositivo de Windows 10 es diferente de las versiones anteriores  
En Windows 10 y AD FS 2016 hay algunos aspectos nuevos del registro y la autenticación de dispositivos que debe conocer (especialmente si está familiarizado con el registro de dispositivos y la "Unión al área de trabajo" en las versiones anteriores).  

En primer lugar, en Windows 10 y AD FS en Windows Server 2016, el registro y la autenticación de dispositivos ya no se basan únicamente en un certificado de usuario X509.  Existe un protocolo nuevo y más sólido que proporciona una mejor seguridad y una experiencia de usuario más sencilla.  Las principales diferencias son que, para la Unión a un dominio de Windows 10 y Azure AD combinación, hay un certificado de equipo X509 y una nueva credencial denominada PRT.  Puede leer toda la información [aquí](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) y [aquí](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

En segundo lugar, Windows 10 y AD FS 2016 admiten la autenticación de usuarios mediante Microsoft Passport for Work, que puede leer [aquí](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) y [aquí](/windows/security/identity-protection/hello-for-business/hello-identity-verification).  

AD FS 2016 proporciona un SSO de usuario y dispositivo sin problemas basado en las credenciales PRT y Passport.  Con los pasos descritos en este documento, puede habilitar estas funcionalidades y verlas.  

### <a name="device-access-control-policies"></a>Directivas de Access Control de dispositivos  
Los dispositivos se pueden usar en reglas de control de acceso AD FS simples, como:  

- permitir el acceso solo desde un dispositivo registrado   
- requerir multi-factor Authentication cuando un dispositivo no está registrado  

Estas reglas se pueden combinar con otros factores, como la ubicación de acceso a la red y la autenticación multifactor, la creación de directivas de acceso condicional enriquecidas como:  


- requerir multi-factor Authentication para dispositivos no registrados que acceden desde fuera de la red corporativa, excepto para los miembros de un grupo o grupos determinados  

Con AD FS 2016, estas directivas se pueden configurar específicamente para requerir un nivel de confianza de dispositivo determinado también: **autenticado**, **administrado**o **compatible**.  

Para obtener más información sobre cómo configurar las directivas de control de acceso de AD FS, consulte [directivas de control de acceso en AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivos autenticados  
Los dispositivos autenticados son dispositivos registrados que no están inscritos en MDM (Intune y MDMs de terceros para Windows 10, solo Intune para iOS y Android).   

Los dispositivos autenticados tendrán la **ismanaged (** AD FS claim con el valor **false**. (Mientras que los dispositivos que no están registrados en absoluto, carecen de esta demanda).  Los dispositivos autenticados (y todos los dispositivos registrados) tendrán la isKnown AD FS claim con el valor **true**.  

#### <a name="managed-devices"></a>Dispositivos administrados:   

Los dispositivos administrados son dispositivos registrados que están inscritos con MDM.  

Los dispositivos administrados tendrán la IsManaged (AD FS claim con el valor **true**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivos compatibles (con MDM o directivas de grupo)  
Los dispositivos compatibles son dispositivos registrados que no solo están inscritos con MDM pero que cumplen con las directivas de MDM. (La información de cumplimiento se origina con MDM y se escribe en Azure AD).  

Los dispositivos compatibles tendrán la **isCompliant** AD FS claim con el valor **true**.    

Para obtener una lista completa de las notificaciones de acceso condicional y de dispositivo AD FS 2016, consulte [referencia](#reference).  


## <a name="reference"></a>Referencia  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Lista completa de nuevas AD FS 2016 y notificaciones de dispositivo  

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
