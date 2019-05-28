---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: Configuración de inicio de sesión único de AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f3719277c80eae2bf2a4d923146920d17546601d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188732"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS único inicio de sesión en la configuración

Inicio de sesión único (SSO) permite a los usuarios autenticarse una vez y tener acceso a varios recursos sin que se le pidan credenciales adicionales.  Este artículo describe el comportamiento de AD FS predeterminado para el inicio de sesión único, así como los valores de configuración que le permiten personalizar este comportamiento.  

## <a name="supported-types-of-single-sign-on"></a>Tipos admitidos de Single Sign-On

AD FS admite varios tipos de experiencias de inicio de sesión único:  
  
-   **Sesión de inicio de sesión único**  
  
     Se escriben las cookies de inicio de sesión único de sesión del usuario autenticado, lo que elimina los mensajes adicionales cuando el usuario cambia las aplicaciones durante una sesión determinada. Sin embargo, si se finaliza una sesión determinada, el usuario se pedirá sus credenciales de nuevo.  
  
     AD FS establecerá sesión las cookies de SSO de forma predeterminada si no se registran los dispositivos de los usuarios. Si la sesión del explorador ha finalizado y se reinicia, esta cookie de sesión se elimina y más no es válida.  
  
-   **Inicio de sesión único persistente**  
  
     Se escriben las cookies persistentes de inicio de sesión único para el usuario autenticado, lo que elimina aún más mensajes cuando el usuario cambia las aplicaciones para siempre y cuando la cookie de inicio de sesión único persistente es válida. La diferencia entre persistente SSO y la sesión de inicio de sesión único es que puede mantenerse el inicio de sesión único persistente entre distintas sesiones.  
  
     AD FS establecerá las cookies persistentes de inicio de sesión único si el dispositivo está registrado. AD FS también establecerá una cookie persistente de inicio de sesión único si el usuario selecciona la opción "mantener la sesión iniciada". Si la cookie de inicio de sesión único persistente más no es válida, se rechaza y se eliminará.  
  
-   **SSO específico de aplicación**  
  
     En el escenario de OAuth, se usa un token de actualización para mantener el estado de inicio de sesión único del usuario dentro del ámbito de una aplicación determinada.  
  
     Si se registra un dispositivo, AD FS establece la hora de expiración de un token de actualización según la duración de las cookies de inicio de sesión único persistente para un dispositivo registrado que es de 7 días de forma predeterminada hasta un máximo de 90 días con AD FS 2016 y 2012 R2 AD FS si usan sus dispositivos acceso a recursos de AD FS durante un período de 14 días. 

Si el dispositivo no está registrado, pero un usuario selecciona la opción "mantener la sesión iniciada", será igual a la hora de expiración del token de actualización de la duración de las cookies de inicio de sesión único persistente para "mantener la que sesión iniciada" que es 1 día de forma predeterminada con el máximo de 7 días. En caso contrario, la vigencia del token es igual a sesión SSO cookie duración de la actualización que es 8 horas de forma predeterminada  
  
 Como se mencionó anteriormente, los usuarios en los dispositivos registrados siempre obtendrá un inicio de sesión único persistente a menos que el inicio de sesión único persistente está deshabilitado. Dispositivos no registrados, inicio de sesión único persistente puede lograrse habilitando la "mantener la sesión iniciada" característica (KMSI). 
 
 Para Windows Server 2012 R2, para habilitar PSSO para el escenario "Mantener la sesión iniciada", debe instalar esta [revisión](https://support.microsoft.com/en-us/kb/2958298/) que también es parte de la de [paquete acumulativo de actualizaciones de agosto de 2014 para Windows RT 8.1, Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   

Tarea | PowerShell | Descripción
------------ | ------------- | -------------
Habilitar o deshabilitar el inicio de sesión único persistente | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| Inicio de sesión único persistente está habilitada de forma predeterminada. Si está deshabilitado, no se escribirá ninguna cookie PSSO.
"Habilitar o deshabilitar"mantener la sesión iniciada" | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | Característica "Mantener la sesión iniciada" está deshabilitada de forma predeterminada. Si está habilitado, usuario final verá una opción "mantener la sesión iniciada" en la página de inicio de sesión de AD FS



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016 - dispositivos autenticados y Single Sign-On
AD FS 2016 cambia el PSSO si el solicitante se autentica desde un dispositivo registrado aumentar al máximo 90 días, pero que requieren un authenticvation dentro de un período de 14 días (ventana de uso de dispositivos).
Después de proporcionar las credenciales por primera vez, de forma predeterminada a los usuarios con dispositivos registrados obtención inicio de sesión único para un período máximo de 90 días, suponiendo que usen el dispositivo para tener acceso a los recursos de AD FS con al menos una vez cada 14 días.  Si se esperan de 15 días después de proporcionar las credenciales, se pedirá a los usuarios las credenciales de nuevo.  

Inicio de sesión único persistente está habilitada de forma predeterminada. Si está deshabilitado, no se escribirá ninguna cookie PSSO. |  

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

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantener la sesión iniciada para dispositivos no autenticados. 
Para los dispositivos no registrados, el período de inicio de sesión único viene determinada por la **mantener Me inicien sesión en (KMSI)** la configuración de características.  KMSI está deshabilitada de forma predeterminada y se puede habilitar estableciendo la propiedad de AD FS KmsiEnabled en True.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Con KMSI deshabilitado, el período predeterminado único inicio de sesión es 8 horas.  Esto se puede configurar mediante la propiedad SsoLifetime.  La propiedad se mide en minutos, por lo que su valor predeterminado es 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Con KMSI habilitado, el período predeterminado único inicio de sesión es de 24 horas.  Esto se puede configurar mediante la propiedad KmsiLifetimeMins.  La propiedad se mide en minutos, por lo que su valor predeterminado es 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamiento de la autenticación multifactor (MFA)  
Es importante tener en cuenta que, proporcionando relativamente largos períodos de inicio de sesión único en AD FS le solicitará la autenticación adicional (autenticación multifactor) cuando un inicio de sesión anterior se basaba en las credenciales principales y no MFA, pero el inicio de sesión actual requiere MFA.  Esto es así independientemente de la configuración de SSO. En primer lugar, AD FS, cuando recibe una solicitud de autenticación, determina si hay un contexto de inicio de sesión único (por ejemplo, una cookie) y, a continuación, si se requiere MFA (por ejemplo, si la solicitud proviene de fuera) evaluarán si el contexto SSO contiene MFA.  Si no es así, se le pedirá MFA.  


  
## <a name="psso-revocation"></a>Revocación PSSO  
 Para proteger la seguridad, AD FS rechazará cualquier cookie persistente de SSO emitido anteriormente cuando se cumplen las condiciones siguientes. Esto requerirá al usuario que proporcione sus credenciales para autenticarse con AD FS de nuevo. 
  
-   Usuario cambia la contraseña  
  
-   Configuración de inicio de sesión único persistente está deshabilitada en AD FS  
  
-   Dispositivo está deshabilitado por el administrador en caso de pérdida o robo  
  
-   AD FS recibe una cookie persistente de SSO que se emitió para un usuario registrado, pero el usuario o el dispositivo no está registrado ya  
  
-   AD FS recibe una cookie persistente de inicio de sesión único para un usuario registrado, pero el usuario volver a registrar  
  
-   AD FS recibe una cookie persistente de SSO que se emite como resultado "mantener la sesión iniciada" pero "mantener la sesión iniciada" se deshabilita la configuración de AD FS  
  
-   AD FS recibe una cookie persistente de SSO que se emitió para un usuario registrado pero el certificado de dispositivo se encuentra o modificado durante la autenticación  
  
-   Administrador de AD FS estableció una hora límite para el inicio de sesión único persistente. Cuando esto se configura, AD FS rechazará cualquier cookie persistente de SSO emitido antes de esta hora  
  
 Para establecer la hora límite, ejecute el siguiente cmdlet de PowerShell:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Habilitar PSSO para los usuarios de Office 365 tener acceso a SharePoint Online  
 Una vez PSSO está habilitado y configurado en AD FS, AD FS escribirá una cookie persistente después de que un usuario autenticado. La próxima vez que el usuario entra en acción, si una cookie persistente es aún válida, un usuario no tiene que proporcionar credenciales para autenticarse de nuevo. También puede evitar el aviso de autenticación adicionales para Office 365 y SharePoint Online a los usuarios mediante la configuración de los siguientes dos reglas en AD FS para la persistencia de desencadenador en Microsoft Azure AD y SharePoint Online de notificaciones.  Para habilitar PSSO para los usuarios de Office 365 acceder a SharePoint online, necesita instalar esta [revisión](https://support.microsoft.com/en-us/kb/2958298/) que también es parte de la de [paquete acumulativo de actualizaciones de agosto de 2014 para Windows RT 8.1, Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Una regla de transformación de emisión para pasar a través de la notificación InsideCorporateNetwork  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "http://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
Para resumir:
<table>
  <tr>
    <th colspan="1">Experiencia de inicio de sesión único</th>
    <th colspan="3">ADFS 2012 R2 <br> ¿Está registrado el dispositivo?</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> ¿Está registrado el dispositivo?</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>NO</th>
    <th>NO, pero KMSI</th>
    <th>SÍ</th>
    <th></th>
    <th>NO</th>
    <th>NO, pero KMSI</th>
    <th>SÍ</th>
  </tr>
 <tr align="center">
    <td>SSO = > establece el Token de actualización = ></td>
    <td>8 horas</td>
    <td>N/D</td>
    <td>N/D</td>
    <th></th>
    <td>8 horas</td>
    <td>N/D</td>
    <td>N/D</td>
  </tr>

 <tr align="center">
    <td>PSSO = > establece el Token de actualización = ></td>
    <td>N/D</td>
    <td>24 horas</td>
    <td>7 días</td>
    <th></th>
    <td>N/D</td>
    <td>24 horas</td>
    <td>Máximo 90 días con la ventana de 14 días</td>
  </tr>

 <tr align="center">
    <td>Duración del token</td>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
    <th></th>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
  </tr>
</table>

**¿Dispositivo registrado?** Obtener un PSSO / inicio de sesión único persistente <br>
**¿Dispositivo no está registrado?** Obtener un inicio de sesión único <br>
**¿Dispositivo no está registrado, pero KMSI?** Obtener un PSSO / inicio de sesión único persistente <p>
IF:
 - [x] administrador ha habilitado la característica KMSI [y]
 - [x] usuario hace clic en la casilla para KMSI en la página de inicio de sesión de formularios
 
**Bueno, a saber:** <br>
Los usuarios federados que no tiene el **LastPasswordChangeTimestamp** atributo sincronizado se emiten cookies de sesión y tokens de actualización que tienen un **valor Max Age de 12 horas**.<br>
Esto ocurre porque Azure AD no puede determinar cuándo se deben revocar los tokens que están relacionados con una credencial antigua (por ejemplo, una contraseña que se ha cambiado). Por lo tanto, Azure AD debe comprobar con más frecuencia para asegurarse de que el usuario y los tokens asociados están aún en buena reputación.
