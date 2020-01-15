---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: Configuración de inicio de sesión único de AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 76c34dc518f4578b4ae2ead3459f1d79c191b3d7
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949195"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS la configuración de inicio de sesión único

El inicio de sesión único (SSO) permite a los usuarios autenticarse una vez y acceder a varios recursos sin que se le soliciten credenciales adicionales.  En este artículo se describe el comportamiento de AD FS predeterminado para SSO, así como los valores de configuración que le permiten personalizar este comportamiento.  

## <a name="supported-types-of-single-sign-on"></a>Tipos admitidos del inicio de sesión único

AD FS admite varios tipos de experiencias de inicio de sesión único:  
  
-   **SSO de sesión**  
  
     Las cookies de SSO de sesión se escriben para el usuario autenticado, lo que elimina los mensajes adicionales cuando el usuario cambia las aplicaciones durante una sesión determinada. Sin embargo, si una sesión determinada finaliza, se le solicitarán sus credenciales de nuevo al usuario.  
  
     De forma predeterminada, AD FS establecerá las cookies de SSO de sesión si no se registran los dispositivos de los usuarios. Si la sesión del explorador ha finalizado y se ha reiniciado, esta cookie de sesión se elimina y no es válida.  
  
-   **SSO persistente**  
  
     Las cookies de SSO persistentes se escriben para el usuario autenticado, lo que elimina los mensajes adicionales cuando el usuario cambia de aplicación mientras la cookie de SSO persistente sea válida. La diferencia entre SSO persistente y SSO de sesión es que el SSO persistente puede mantenerse en distintas sesiones.  
  
     AD FS establecerá cookies de SSO persistentes si el dispositivo está registrado. AD FS también establecerá una cookie de SSO persistente si un usuario selecciona la opción "mantener la sesión iniciada". Si la cookie de SSO persistente no es válida, se rechazará y se eliminará.  
  
-   **SSO específico de la aplicación**  
  
     En el escenario de OAuth, se usa un token de actualización para mantener el estado de SSO del usuario dentro del ámbito de una aplicación determinada.  
  
     Si se registra un dispositivo, AD FS establecerá la hora de expiración de un token de actualización en función de la duración de las cookies de SSO persistentes para un dispositivo registrado que tenga siete días de forma predeterminada para AD FS 2012R2 y hasta un máximo de 90 días con AD FS 2016 si usan su dispositivo para acceder a AD FS recursos en un período de 14 días. 

Si el dispositivo no está registrado pero un usuario selecciona la opción "mantener la sesión iniciada", la fecha de expiración del token de actualización será igual a la duración de las cookies de SSO persistentes para "mantener la sesión iniciada", que es 1 día de forma predeterminada con un máximo de 7 días. De lo contrario, la vigencia del token de actualización es igual a la duración de la cookie de SSO de sesión, que es de 8 horas  
  
 Como se mencionó anteriormente, los usuarios de los dispositivos registrados siempre obtendrán un SSO persistente a menos que el SSO persistente esté deshabilitado. En el caso de los dispositivos no registrados, se puede lograr SSO persistente habilitando la característica "mantener la sesión iniciada" (KMSI). 
 
 En Windows Server 2012 R2, para habilitar PSSO para el escenario "mantener la sesión iniciada", debe instalar esta [revisión](https://support.microsoft.com/kb/2958298/) , que también forma parte del [paquete acumulativo de actualizaciones de agosto de 2014 para Windows RT 8,1, Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/kb/2975719).   

Tarea | PowerShell | Descripción
------------ | ------------- | -------------
Habilitar o deshabilitar SSO persistente | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| SSO persistente está habilitado de forma predeterminada. Si está deshabilitada, no se escribirá ninguna cookie PSSO.
"Habilitar/deshabilitar" mantener la sesión iniciada " | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | La característica "mantener la sesión iniciada" está deshabilitada de forma predeterminada. Si está habilitada, el usuario final verá una opción "mantener la sesión iniciada" en AD FS página de inicio de sesión



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016: Inicio de sesión único y dispositivos autenticados
AD FS 2016 cambia el PSSO cuando el solicitante realiza la autenticación desde un dispositivo registrado que se incrementa hasta un máximo de 90 días, pero requiere una autenticación en un período de 14 días (ventana uso del dispositivo).
Después de proporcionar las credenciales por primera vez, de forma predeterminada, los usuarios con dispositivos registrados obtienen un inicio de sesión único durante un período máximo de 90 días, siempre que usen el dispositivo para tener acceso a AD FS recursos al menos una vez cada 14 días.  Si esperan 15 días después de proporcionar las credenciales, se solicitarán de nuevo a los usuarios las credenciales.  

SSO persistente está habilitado de forma predeterminada. Si está deshabilitada, no se escribirá ninguna cookie PSSO. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
La ventana uso del dispositivo (14 días de forma predeterminada) se rige por la propiedad AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
El período de inicio de sesión único máximo (90 días de forma predeterminada) se rige por la propiedad AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantener la sesión iniciada en dispositivos no autenticados 
En el caso de los dispositivos no registrados, el período de inicio de sesión único viene determinado por la configuración de la característica **mantener la sesión iniciada (KMSI)** .  KMSI está deshabilitado de forma predeterminada y se puede habilitar estableciendo la propiedad de la AD FS KmsiEnabled en true.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Con KMSI deshabilitado, el período de inicio de sesión único predeterminado es de 8 horas.  Se puede configurar mediante la propiedad SsoLifetime.  La propiedad se mide en minutos, por lo que su valor predeterminado es 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Con KMSI habilitado, el período de inicio de sesión único predeterminado es de 24 horas.  Se puede configurar mediante la propiedad KmsiLifetimeMins.  La propiedad se mide en minutos, por lo que su valor predeterminado es 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamiento de multi-factor Authentication (MFA)  
Es importante tener en cuenta que, a la vez que se proporcionan períodos relativamente largos de inicio de sesión único, AD FS solicitará autenticación adicional (autenticación multifactor) cuando un inicio de sesión anterior se basaba en las credenciales principales y no en MFA, pero el inicio de sesión actual requiere MFA.  Esto es independiente de la configuración de SSO. AD FS, cuando recibe una solicitud de autenticación, primero determina si hay un contexto de SSO (como una cookie) y, a continuación, si se requiere MFA (por ejemplo, si la solicitud está llegando desde fuera), se evaluará si el contexto de SSO contiene MFA.  Si no es así, se le solicitará MFA.  


  
## <a name="psso-revocation"></a>Revocación de PSSO  
 Para proteger la seguridad, AD FS rechazará todas las cookies de SSO persistentes que se hayan emitido anteriormente cuando se cumplan las siguientes condiciones. Esto requerirá que el usuario proporcione sus credenciales para autenticarse con AD FS de nuevo. 
  
- El usuario cambia la contraseña  
  
- La configuración de SSO persistente está deshabilitada en AD FS  
  
- El administrador deshabilitó el dispositivo en caso de pérdida o robo  
  
- AD FS recibe una cookie de SSO persistente que se emite para un usuario registrado pero el usuario o el dispositivo ya no está registrado  
  
- AD FS recibe una cookie de SSO persistente para un usuario registrado, pero el usuario se ha vuelto a registrar  
  
- AD FS recibe una cookie de SSO persistente que se emite como resultado de "mantener la sesión iniciada" pero está deshabilitada la opción "mantener la sesión iniciada" en AD FS  
  
- AD FS recibe una cookie de SSO persistente que se emite para un usuario registrado, pero el certificado de dispositivo falta o se modifica durante la autenticación  
  
- AD FS administrador ha establecido un límite de tiempo para SSO persistente. Cuando se configura, AD FS rechazará todas las cookies de SSO persistentes emitidas antes de esta hora.  
  
  Para establecer el tiempo límite, ejecute el siguiente cmdlet de PowerShell:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Habilitación de PSSO para que los usuarios de Office 365 tengan acceso a SharePoint Online  
 Una vez que PSSO está habilitado y configurado en AD FS, AD FS escribirá una cookie persistente después de que un usuario se haya autenticado. La próxima vez que el usuario entra en, si una cookie persistente sigue siendo válida, no es necesario que el usuario proporcione credenciales para autenticarse de nuevo. También puede evitar el aviso de autenticación adicional para los usuarios de Office 365 y SharePoint Online mediante la configuración de las dos siguientes reglas de notificaciones en AD FS para desencadenar la persistencia en Microsoft Azure AD y SharePoint Online.  Para permitir que los usuarios de PSSO para Office 365 tengan acceso a SharePoint Online, debe instalar esta [revisión](https://support.microsoft.com/kb/2958298/) , que también forma parte del [paquete acumulativo de actualizaciones de agosto de 2014 para Windows RT 8,1, Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/kb/2975719).  
  
 Una regla de transformación de emisión para pasar a través de la demanda de InsideCorporateNetwork  
  
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
  
En Resumen:
<table>
  <tr>
    <th colspan="1">Experiencia de inicio de sesión único</th>
    <th colspan="3">ADFS 2012 R2 <br> ¿El dispositivo está registrado?</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> ¿El dispositivo está registrado?</th>
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
    <td>SSO =&gt;establecer token de actualización =&gt;</td>
    <td>8 horas</td>
    <td>N/A</td>
    <td>N/A</td>
    <th></th>
    <td>8 horas</td>
    <td>N/A</td>
    <td>N/A</td>
  </tr>

 <tr align="center">
    <td>PSSO =&gt;establecer token de actualización =&gt;</td>
    <td>N/A</td>
    <td>24 horas</td>
    <td>7 días</td>
    <th></th>
    <td>N/A</td>
    <td>24 horas</td>
    <td>Ventana máx. 90 días con 14 días</td>
  </tr>

 <tr align="center">
    <td>Vigencia del token</td>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
    <th></th>
    <td>1 h</td>
    <td>1 h</td>
    <td>1 h</td>
  </tr>
</table>

**¿El dispositivo está registrado?** Obtiene un SSO PSSO/persistente. <br>
**¿No se ha registrado el dispositivo?** Obtiene un inicio de sesión único (SSO) <br>
**¿No se ha registrado el dispositivo pero KMSI?** Obtiene un SSO PSSO/persistente. <p>
Cuando
 - [x] el administrador ha habilitado la característica KMSI [y]
 - [x] el usuario hace clic en la casilla KMSI en la página de inicio de sesión de formularios.
 
**Bueno para saberlo:** <br>
Los usuarios federados que no tengan el atributo **LastPasswordChangeTimestamp** sincronizado reciben cookies de sesión y tokens de actualización que tienen un **valor de antigüedad máximo de 12 horas**.<br>
Esto se debe a que Azure AD no puede determinar cuándo revocar los tokens relacionados con una credencial anterior (como una contraseña que se ha cambiado). Por lo tanto, Azure AD debe comprobar con mayor frecuencia para asegurarse de que el usuario y los tokens asociados todavía están en buen estado.
