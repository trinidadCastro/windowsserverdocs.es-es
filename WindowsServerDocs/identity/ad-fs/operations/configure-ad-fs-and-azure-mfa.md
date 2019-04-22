---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configuración de AD FS 2016 y Azure MFA
description: ''
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ae7809089a69ac0ff48168db0aa2e9d61c35257a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814096"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>Configure Azure MFA como proveedor de autenticación con AD FS

>Se aplica a: Windows Server 2016, Windows Server 2019

Si su organización está federada con Azure AD, puede usar Azure Multi-factor Authentication para proteger los recursos de AD FS, tanto locales como en la nube. MFA de Azure le permite eliminar las contraseñas y proporcionan una manera más segura de autenticar.  A partir de Windows Server 2016, ahora puede configurar Azure MFA para la autenticación principal o utilizarlo como un proveedor de autenticación adicional. 
  
A diferencia de con AD FS en Windows Server 2012 R2, el adaptador de AD FS 2016 Azure MFA se integra directamente con Azure AD y no requiere un servidor Azure MFA local.   El adaptador de MFA de Azure está integrado en Windows Server 2016, y no es necesario para una instalación adicional.


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>Registro de usuarios para Azure MFA con AD FS

AD FS no se admite en línea &#34;prueba seguridad&#34;, o el registro de información de comprobación de seguridad de Azure MFA como número de teléfono o aplicación móvil. Esto significa que los usuarios deben obtener poseer una protección de visitando [ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx) antes de usar Azure MFA para autenticarse en aplicaciones de AD FS. Cuando un usuario no ha disponibilidad aún copia en Azure AD intenta autenticarse con Azure MFA en AD FS, producirá un error de AD FS.  Como administrador de AD FS, puede personalizar esta experiencia de error para guiar al usuario a la página de proofup, en su lugar.  Puede hacerlo mediante onload.js personalización para detectar la cadena de mensaje de error en la página de AD FS y mostrar un mensaje nuevo para guiar a los usuarios visitar [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup), a continuación, vuelva a intentar la autenticación. Para obtener instrucciones detalladas, consulte la "AD FS web página Personalizar guiar a los usuarios para registrar los métodos de verificación de MFA" a continuación en este artículo.

>[!NOTE]
> Anteriormente, eran necesario a los usuarios para autenticarse con MFA para el registro (visitar [ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx), por ejemplo, mediante el método abreviado [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)).  Ahora, un usuario de AD FS que todavía no registró la información de comprobación de MFA puede acceder a Azure AD&#34;página de proofup s mediante el método abreviado [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) utilizando únicamente la autenticación principal (por ejemplo, la autenticación integrada de Windows o nombre de usuario y contraseña a través de la instancia de AD FS páginas web).  Si el usuario no tiene ningún método de verificación configurado, Azure AD realizará el registro en línea en el que el usuario ve el mensaje &#34;el administrador requiere que configure esta cuenta para la comprobación de seguridad adicional&#34;, y el usuario puede, a continuación, Seleccione esta opción para &#34;configurarlo ahora&#34;.
> Los usuarios que ya tienen al menos un método de verificación de MFA configurado todavía le pedirá que proporcione de MFA cuando se visita la página de proofup.

## <a name="recommended-deployment-topologies"></a>Topologías de implementación recomendadas

### <a name="azure-mfa-as-primary-authentication"></a>Azure MFA como autenticación principal

Hay un par de buenas razones para usar Azure MFA como autenticación principal con AD FS:

 - Para evitar las contraseñas para iniciar sesión en Azure AD, Office 365 y otras aplicaciones de AD FS
 - Para proteger la contraseña basado en sesión requiriendo un factor adicional como el código de comprobación antes de la contraseña

Si desea usar Azure MFA como método de autenticación principal en AD FS para lograr estos beneficios, probablemente también querrá mantener la capacidad de usar acceso condicional de Azure AD, incluidas &#34;true MFA&#34; solicitando factores adicionales en AD FS.

Ahora puede hacerlo mediante la configuración de la configuración de dominio de Azure AD para realizar la MFA en el entorno local (configuración &#34;SupportsMfa&#34; en $True).  En esta configuración, AD FS se puede solicitar que Azure AD para realizar la autenticación adicional o &#34;true MFA&#34; para escenarios de acceso condicional que lo requieran.  

Como se describió anteriormente, cualquier usuario de AD FS que aún no ha registrado (información de comprobación de MFA configurada) debe consultarse a través de una página de error personalizada de AD FS para visitar [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) para configurar la información de comprobación, a continuación, Vuelva a intentar el inicio de sesión de AD FS.  
Dado que Azure MFA como principal se considera un solo factor, después de que los usuarios de la configuración inicial debe proporcionar un factor adicional para administrar o actualizar su información de comprobación de Azure AD, o para tener acceso a otros recursos que requieren MFA.

>[!NOTE]
> Con ADFS de 2019, son necesarios para realizar una modificación en el tipo de notificación delimitador para la confianza de proveedor de notificaciones de Active Directory y modificar desde el windowsaccountname a UPN. Ejecute el siguiente commandlet de powershell proporcionada a continuación. Esto no influye en el funcionamiento interno de la granja de servidores de AD FS. Es posible que observe que algunos usuarios pueden se introducir de nuevo las credenciales una vez que se realiza este cambio. Después de iniciar sesión de nuevo, los usuarios finales no verán ninguna diferencia. 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA como autenticación adicional para Office 365

Anteriormente, si desea realizar Azure MFA como método de autenticación adicional en AD FS de Office 365 o de otros usuarios de confianza, la mejor opción era para configurar Azure AD para compuesta de MFA, en el que se realiza la autenticación principal en el entorno local de AD FS y MFA es tr iggered por Azure AD. Ahora, puede usar Azure MFA como autenticación adicional en AD FS cuando la configuración del dominio SupportsMfa está establecida en $True.  

Como se describió anteriormente, cualquier usuario de AD FS que aún no ha registrado (información de comprobación de MFA configurada) debe consultarse a través de una página de error personalizada de AD FS para visitar [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) para configurar la información de comprobación, a continuación, Vuelva a intentar el inicio de sesión de AD FS.  

## <a name="pre-requisites"></a>Requisitos previos

Los siguientes requisitos previos son necesarios para usar Azure MFA para la autenticación con AD FS:  
  
- Un [suscripción de Azure con Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Microsoft Azure Multi-factor Authentication](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  
- Proxy de aplicación Web es capaz de communticate con lo siguiente sobre los puertos 80 y 443

    - https://adnotifications.windowsazure.com
    - https://login.microsoftonline.com


> [!NOTE]
> Azure AD y Azure MFA se incluyen en Azure AD Premium y Enterprise Mobility Suite (EMS).  Si tiene alguna de estas suscripciones individuales no es necesario.
- Un entorno local de AD FS en Windows Server 2016.  
   - El servidor debe ser capaz de comunicarse con las siguientes direcciones URL a través de los puertos 80 y 443.
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- El entorno local es [federada con Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Módulo de Azure Active Directory de Windows para Windows PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).  
- Permisos de administrador global en su instancia de Azure AD para configurarlo con Azure AD PowerShell.  
- Credenciales de administrador de empresa para configurar la granja de AD FS para Azure MFA.  
  
## <a name="configure-the-ad-fs-servers"></a>Configurar los servidores de AD FS

Para completar la configuración para Azure MFA para AD FS, deberá configurar cada servidor de AD FS mediante los pasos descritos. 

>[!NOTE]
>Asegúrese de que estos pasos se realizan en **todas** servidores de AD FS en la granja de servidores. Si tiene varios servidores de AD FS en la granja de servidores, puede realizar la configuración necesaria de forma remota con Azure AD Powershell.  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Paso 1: Generar un certificado para Azure MFA en cada servidor de AD FS mediante el `New-AdfsAzureMfaTenantCertificate` cmdlet

Lo primero que debe hacer es generar un certificado para Azure MFA para usar.  Esto puede hacerse mediante PowerShell.  El certificado generado puede encontrarse en el almacén de certificados de equipos locales y está marcado con un nombre de sujeto que contiene el valor de TenantID para su directorio Azure AD.

![MFA y AD FS](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

Tenga en cuenta que TenantID es el nombre del directorio de Azure AD.  Use el siguiente cmdlet de PowerShell para generar el nuevo certificado.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![MFA y AD FS](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>Paso 2: Agregue las nuevas credenciales para la entidad de servicio del cliente de Azure Multi-factor Auth

Para permitir que los servidores de AD FS para comunicarse con el cliente de autenticación multifactor de Azure, deberá agregar las credenciales para la entidad de servicio para el cliente de autenticación multifactor de Azure. Los certificados generados mediante la `New-AdfsAzureMFaTenantCertificate` cmdlet le servirá de estas credenciales. Realice el siguiente uso de PowerShell para agregar las nuevas credenciales para la entidad de servicio del cliente de Azure Multi-factor Auth.  

> [!NOTE]
> Para completar este paso deberá conectarse a la instancia de Azure AD con PowerShell mediante Connect-MsolService.  Estos pasos se supone que ya se ha conectado a través de PowerShell.  Para obtener información, consulte [Connect-MsolService.](https://msdn.microsoft.com/library/dn194123.aspx)  

**Establezca el certificado como la nueva credencial en el cliente de autenticación multifactor de Azure**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> Este comando debe ejecutarse en todos los servidores de AD FS en la granja de servidores.  MFA de Azure AD se producirá un error en los servidores que no tiene el certificado establecido como la nueva credencial en el cliente de autenticación multifactor de Azure.

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 es el GUID de cliente de autenticación multifactor de Azure.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurar la granja de servidores de AD FS  
  
Una vez haya completado la sección anterior en cada servidor de AD FS, deberá ejecutar el `Set-AdfsAzureMfaTenant` cmdlet.  
  
Este cmdlet debe ejecutarse una sola vez para una granja de AD FS.  Usar PowerShell para completar este paso.
  
> [!NOTE]  
> Deberá reiniciar el servicio de AD FS en cada servidor en la granja de servidores para que estos cambios surtan efecto.  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  

![MFA y AD FS](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
Una vez hecho esto, verá que Azure MFA está disponible como un método de autenticación principal para la intranet y extranet uso.    
  
![MFA y AD FS](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Renovar y administrar AD FS Azure MFA certificados

Las siguientes instrucciones le explicamos cómo administrar los certificados de Azure MFA en los servidores de AD FS.
De forma predeterminada, cuando se configura AD FS con Azure MFA, los certificados generados mediante el cmdlet New-AdfsAzureMfaTenantCertificate PowerShell son válidos durante 2 años.  Para determinar cómo cerrar para expiración de los certificados no están y, a continuación, para renovar e instalar certificados nuevos, use el procedimiento siguiente.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Evaluar la fecha de expiración del certificado de AD FS Azure MFA

En cada servidor de AD FS, en el equipo local Mi almacén, habrá un certificado autofirmado con &#34;OU = Microsoft AD FS Azure MFA&#34; en el emisor y el asunto.  Este es el certificado de Azure MFA.  Compruebe que el período de validez de este certificado en cada servidor de AD FS para determinar la fecha de expiración.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Crear nuevo un certificado de MFA de Azure de AD FS en cada servidor de AD FS

Si el período de validez de los certificados está llegando al final, inicie el proceso de renovación mediante la generación de un nuevo certificado de Azure MFA en cada servidor de AD FS. En una ventana de comandos de powershell, generar un nuevo certificado en cada servidor de AD FS mediante el siguiente cmdlet:

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

Como resultado de este cmdlet, se generará un nuevo certificado válido de 2 días en el futuro a 2 días más de 2 años.  Las operaciones de AD FS y Azure MFA no se verán afectadas por este cmdlet o el nuevo certificado. (Nota: el retraso de dos días es intencionado y proporciona tiempo para ejecutar los pasos siguientes para configurar el nuevo certificado en el inquilino antes de que AD FS comienza a usarla para Azure MFA.)

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configure cada nuevo certificado de AD FS Azure MFA en el inquilino de Azure AD

Con el módulo de PowerShell de Azure AD, para cada nuevo certificado (en cada servidor de AD FS), actualice la configuración de inquilino de Azure AD como sigue (Nota: debe conectarse primero en el inquilino con Connect-MsolService para ejecutar los comandos siguientes).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

$certbase64 es el nuevo certificado.  El certificado codificado en base64 que puede obtenerse mediante la exportación del certificado (sin la clave privada) como un DER codificado el archivo y de apertura en Notepad.exe, a continuación, copie y peque a la sesión PSH y asignar a la variable $certbase64

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Compruebe que se utilizará los nuevos certificados para Azure MFA

Una vez que los nuevos certificados que se van a ser válidos, AD FS se seleccionará y comience a usar cada certificado correspondiente para Azure MFA en unas horas en un día.  Una vez que esto ocurre, en cada servidor se visualizará un evento en el registro de eventos de administración de AD FS con la siguiente información: Nombre de registro:      Origen de AD FS/Admin:        Fecha de AD FS:          27/2/2018 Id. de evento 7:33:31 PM:      Categoría de tarea 547: Ninguno de nivel:         Palabras clave de información:      Usuario de AD FS:          Equipo DOMAIN\adfssvc:      Descripción de ADFS.domain.contoso.com: Se renovó el certificado del inquilino para Azure MFA.  

TenantId: contoso.onmicrosoft.com.
Antigua huella digital: 7CC103D60967318A11D8C51C289EF85214D9FC63.
Fecha de expiración anterior: 9/15/2019 9:43:17 PM.
Nueva huella digital: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF.
Nueva fecha de expiración: 27/2/2020 2:16:07 AM.

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personalizar la página web de AD FS para guiar a los usuarios para registrar los métodos de verificación de MFA

Utilice los siguientes ejemplos personalizar las páginas web de AD FS para los usuarios que no hayan compatible tecnologías aún copia (información de comprobación de MFA configurada).

### <a name="find-the-error"></a>Encuentre el error

En primer lugar, hay un par de AD FS se devolverá en el caso en el que el usuario no tiene información de comprobación de mensajes de error diferentes.
Si usa Azure MFA como autenticación principal, el usuario sin proofed verá una página de error de AD FS que contiene los siguientes mensajes:
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
Cuando se está intentando ejecutar Azure AD como autenticación adicional, el usuario sin proofed verá una página de error de AD FS que contiene los siguientes mensajes:
```
<div id='mfaGreetingDescription' class='groupMargin'>For security reasons, we require additional information to verify your account (mahesh@jenfield.net)</div>
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            The selected authentication method is not available for &#39;username@contoso.com&#39;. Choose another authentication method or contact your system administrator for details.
        </div>
```

### <a name="catch-the-error-and-update-the-page-text"></a>Detectar el error y actualizar el texto de la página

Para detectar el error y muestra al usuario guía personalizada simplemente anexe el código javascript al final del archivo onload.js que forma parte del tema web AD FS.  Esto le permite hacer lo siguiente:
 - Busque las cadenas de error de identificación
 - proporcionar contenido web personalizado.  

(Para obtener consejos en general acerca de cómo personalizar el archivo onload.js, consulte el artículo [personalización avanzada de AD FS Sign-in Pages](advanced-customization-of-ad-fs-sign-in-pages.md).)

Este es un ejemplo sencillo, es posible que desea extender:

1. Abra Windows PowerShell en el servidor de AD FS principal y crear un nuevo tema Web de AD FS, ejecute el comando siguiente:
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. A continuación, exporte el valor predeterminado de tema Web de AD FS:

    ``` PowerShell
       Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. Abra el archivo C:\Theme\script\onload.js en un editor de texto
4. Anexe el código siguiente al final del archivo onload.js
    
    ``` JavaScript
    //Custom Code
    //Customize MFA exception
    //Begin

    var domain_hint = "<YOUR_DOMAIN_NAME_HERE>";
    var mfaSecondFactorErr = "The selected authentication method is not available for";
    var mfaProofupMessage = "You will be automatically redirected in 5 seconds to set up your account for additional security verification. Once you have completed the setup, please return to the application you are attempting to access.<br><br>If you are not redirected automatically, please click <a href='{0}'>here</a>."
    var authArea = document.getElementById("authArea");
    if (authArea) {
        var errorMessage = document.getElementById("errorMessage");
        if (errorMessage.innerHTML.indexOf(mfaSecondFactorErr) >= 0) {

        //Hide the error message
            var openingMessage = document.getElementById("openingMessage");
            if (openingMessage) {
                openingMessage.style.display = 'none'
            }
            var errorDetailsLink = document.getElementById("errorDetailsLink");
            if (errorDetailsLink) {
                errorDetailsLink.style.display = 'none'
            }

            //Provide a message and redirect to Azure AD MFA Registration Url
            var mfaRegisterUrl = "https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1&whr=" + domain_hint;
            errorMessage.innerHTML = "<br>" + mfaProofupMessage.replace("{0}", mfaRegisterUrl);
            window.setTimeout(function () { window.location.href = mfaRegisterUrl; }, 5000);
        }
    }

    //End Customize MFA Exception
    //End Custom Code
    ```
    > [!IMPORTANT]
    > Deberá cambiar "< YOUR_DOMAIN_NAME_HERE >"; Para usar el nombre de dominio. Por ejemplo: `var domain_hint = "contoso.com";`
    
5. Guarde el archivo onload.js
6. Importar el archivo onload.js al tema personalizado escribiendo el siguiente comando de Windows PowerShell:
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}
    ```
7. Por último, aplique el Web tema personalizado de AD FS escribiendo el siguiente comando de Windows PowerShell:
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName
    ```

## <a name="next-steps"></a>Pasos siguientes

[Administrar los protocolos TLS/SSL y conjuntos de cifrado utilizado por AD FS y Azure MFA](manage-ssl-protocols-in-ad-fs.md)
