---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: "AD FS 2016 configuración de sesión único"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e8c24399949efc1b8d0b1782e299593c02374c62
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS único inicio de sesión en la configuración

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Inicio de sesión único (SSO) permite a los usuarios autenticar una vez y tener acceso a varios recursos sin que se le pida credenciales adicionales.  Este artículo describe el comportamiento de AD FS predeterminado para SSO, así como las opciones de configuración que permiten personalizar este comportamiento.  

## <a name="supported-types-of-single-sign-on"></a>Tipos compatibles de inicio de sesión único

AD FS admite varios tipos de inicio de sesión único experiencias:  
  
-   **Sesión de inicio de sesión único**  
  
     Las cookies de sesión SSO se han escrito para el usuario autenticado, lo que elimina los mensajes adicionales cuando el usuario cambia aplicaciones durante una sesión determinada. Sin embargo, si una sesión determinada finaliza, el usuario se pedirá las credenciales de su nuevo.  
  
     AD FS establecerá sesión cookies SSO de manera predeterminada si no se registran los dispositivos de los usuarios. Si la sesión del explorador ha finalizado y se reinicia, esta cookie de sesión se elimina y más no es válida.  
  
-   **SSO persistente**  
  
     Se escriben las cookies persistentes de SSO para el usuario autenticado que aún más elimina mensajes que aparecen cuando el usuario cambia para las aplicaciones como la cookie SSO persistente es válida. La diferencia entre persistente SSO y sesión SSO es que puede mantenerse SSO persistente entre sesiones diferentes.  
  
     AD FS establecen las cookies persistentes de SSO si el dispositivo está registrado. AD FS también establecen una cookie SSO persistente si un usuario selecciona la opción "Mantenerme conectado". Si la cookie SSO persistente más no es válida, se rechaza y se eliminará.  
  
-   **SSO específico de aplicación**  
  
     En el escenario de OAuth, un token de actualización se usa para mantener el estado de inicio de sesión único del usuario dentro del ámbito de una aplicación concreta.  
  
     Si un dispositivo está registrado, AD FS configurará el tiempo de expiración de un token de actualización en función de la duración de las cookies persistente de SSO para un dispositivo registrado, que es 7 días de forma predeterminada. Si un usuario selecciona la opción "Mantenerme conectado", será igual a la hora de expiración del token de actualización de la duración de las cookies persistente de SSO para "mantener iniciado sesión" que es 1 día de forma predeterminada con el máximo de 7 días. De lo contrario, actualizar el token del ciclo de vida es igual a SSO cookie duración de la sesión que es de 8 horas de manera predeterminada  
  
 Como se mencionó anteriormente, los usuarios en los dispositivos registrados siempre obtendrán una SSO persistente salvo el SSO persistente esté deshabilitado. Para los dispositivos no registrados, puede lograrse SSO persistente habilitando la "iniciado sesión mantener" característica (KMSI). 
 
 Para Windows Server 2012 R2, para habilitar PSSO para el escenario "Mantenerme conectado", deberás instalar esto [revisión](https://support.microsoft.com/en-us/kb/2958298/) que también es parte de la de [paquete acumulativo de actualizaciones de agosto de 2014 para Windows RT 8.1, Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   
 
  
## <a name="single-sign-on-and-authenticated-devices"></a>Inicio de sesión único y dispositivos autenticados  
Después de proporcionar credenciales por primera vez, de manera predeterminada a los usuarios con dispositivos registrados Obtén inicio de sesión único durante un período máximo de 90 días, siempre que usan el dispositivo para acceder a recursos de AD FS al menos una vez cada 14 días.  Si esperan 15 días después de proporcionar credenciales, los usuarios se pedirá las credenciales de nuevo.  

SSO persistente está habilitada de manera predeterminada. Si está deshabilitado, no se escribirá ninguna cookie PSSO. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
La ventana de uso del dispositivo (14 días de forma predeterminada) se rige por la propiedad de AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
El único inicio de sesión período máximo (90 días de forma predeterminada) se rige por la propiedad de AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantenerme conectado para dispositivos no autenticados 
Para dispositivos no registrado, el período de inicio de sesión único se determina por el **mantener iniciado en (KMSI)** configuración de características.  KMSI está deshabilitado de manera predeterminada y se puede habilitar estableciendo la propiedad de AD FS KmsiEnabled "true".

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Con KMSI deshabilitado, el período de inicio de sesión único de predeterminado es de 8 horas.  Esto puede configurarse mediante la propiedad SsoLifetime.  La propiedad se mide en minutos, por lo que su valor predeterminado es de 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Con KMSI habilitado, el período de inicio de sesión único de predeterminado es 24 horas.  Esto puede configurarse mediante la propiedad KmsiLifetimeMins.  La propiedad se mide en minutos, por lo que su valor predeterminado es 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamiento de la autenticación multifactor (AMF)  
Es importante tener en cuenta, mientras que proporcionas relativamente largos períodos de inicio de sesión único, AD FS le pedirán autenticación adicional (autenticación de factor de multi) cuando un inicio de sesión anterior se basada en credenciales principales y no MFA, pero sesión actual en requiere MFA.  Esto es independiente de la configuración de inicio de sesión único. En primer lugar, AD FS, cuando recibe una solicitud de autenticación, determina si no hay un contexto de inicio de sesión único (por ejemplo, una cookie) y, a continuación, si es necesario MFA (por ejemplo, si la solicitud proviene de fuera de) evaluará o no el contexto SSO contiene MFA.  De lo contrario, se le pide MFA.  


  
## <a name="psso-revocation"></a>Revocación PSSO  
 Para proteger la seguridad, AD FS rechazará las cookies persistentes de SSO emitidas anteriormente cuando se cumplan las siguientes condiciones. Esto requiere que el usuario proporcionar sus credenciales para volver a autenticar con AD FS. 
  
-   Cambios de contraseña del usuario  
  
-   Configuración de inicio de sesión único persistente está deshabilitada en AD FS  
  
-   Dispositivo está deshabilitado por el administrador en caso de pérdida o robo  
  
-   AD FS recibe una cookie SSO persistente emitido por un usuario registrado pero el usuario o el dispositivo no está registrado ya  
  
-   AD FS recibe una cookie SSO persistente para que un usuario registrado, pero el usuario volver a registrarse  
  
-   AD FS recibe una cookie SSO persistente emitido como resultado "Mantenerme conectado" pero "Mantenerme conectado" configuración está deshabilitada en AD FS  
  
-   AD FS recibe una cookie SSO persistente emitido para un usuario registrado pero el certificado de dispositivo se encuentra o modificado durante la autenticación  
  
-   Administrador de AD FS establece una vez corte para SSO persistente. Cuando esto está configurado, AD FS rechazará las cookies persistentes de SSO emitidas antes de ese momento  
  
 Para establecer la hora del televisor, ejecuta el siguiente cmdlet de PowerShell:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Habilitar PSSO para los usuarios de Office 365 tener acceso a SharePoint Online  
 Una vez PSSO está habilitado y configurado en AD FS, AD FS escribirá una cookie persistente después de que un usuario ha autenticado. La próxima vez que el usuario entra en acción, si una cookie persistente sigue siendo válida, un usuario no tiene que proporcionar credenciales para volver a autenticar. También puede evitar la petición de autenticación adicional para Office 365 y los usuarios de SharePoint Online mediante la configuración de las siguientes dos reclamaciones reglas en AD FS con la persistencia de desencadenador de Microsoft Azure AD y SharePoint Online.  Para habilitar PSSO para los usuarios de Office 365 tener acceso a SharePoint online, deberás instalar esto [revisión](https://support.microsoft.com/en-us/kb/2958298/) que también es parte de la de [paquete acumulativo de actualizaciones de agosto de 2014 para Windows RT 8.1, Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Una regla de emisión de transformación para pasar la reclamación InsideCorporateNetwork  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "https://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
  
    


