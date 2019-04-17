---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurar AD FS 2016 y MFA de Azure
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be8e88ae36f344f1265fb76e66c19e0ac8aeb533
ms.sourcegitcommit: ffdfbb76c7f775e20d089d3f662d4f9a7d642f1e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2018
---
# <a name="configure-ad-fs-2016-and-azure-mfa"></a>Configurar AD FS 2016 y MFA de Azure

>Se aplica a: Windows Server 2016

Si tu organización está federada con Azure AD, puedes usar autenticación multifactor de Azure a los recursos de AD FS seguros, tanto local y en la nube. Azure MFA te permite eliminar las contraseñas y proporcionan una forma más segura para autenticar.  A partir de Windows Server 2016, ahora puedes configurar Azure AMF para la autenticación principal. 
  
A diferencia con AD FS en Windows Server 2012 R2, el adaptador MFA de Azure AD FS 2016 se integra directamente con Azure AD y no requiere un servidor de MFA de Azure de local.   El adaptador de Azure MFA está integrado en Windows Server 2016, y no es necesario para la instalación adicional.

## <a name="registering-users-for-azure-mfa-with-ad-fs-2016"></a>Registro de los usuarios de MFA de Azure con AD FS de 2016
AD FS no se admite en línea "prueba arriba", o registro de información de comprobación de seguridad de Azure AMF como número de teléfono o una aplicación móvil. Esto significa que los usuarios deben obtener compatible tecnologías arriba de visitando https://account.activedirectory.windowsazure.com/Proofup.aspx antes de usar MFA de Azure para autenticarse en aplicaciones de AD FS. Cuando un usuario no ha disponibilidad aún arriba en Azure AD intenta realizar la autenticación con Azure MFA en AD FS, obtendrá un error de AD FS.  Como administrador de AD FS, puedes personalizar esta experiencia de error para guiar al usuario a la página proofup en su lugar.  Puedes hacer esto con personalización de onload.js para detectar la cadena de mensaje de error dentro de la página de AD FS y mostrar un mensaje nuevo para guiar a los usuarios para visitar https://aka.ms/mfasetup y luego volver a intentar la autenticación. Para obtener instrucciones detalladas, consulta la personalizar la AD FS web "página" para guiar a los usuarios para registrar los métodos de comprobación de MFA a continuación en este artículo.

>[!NOTE]
> Anteriormente, los usuarios tenían que autenticar con MFA de registro (visitando https://account.activedirectory.windowsazure.com/Proofup.aspx, por ejemplo mediante el método abreviado aka.ms/mfasetup).  Ahora, un usuario de AD FS que no se ha registrado aún información de comprobación de MFA puede acceder a Azure AD proofup página mediante el método abreviado aka.ms/mfasetup consume autenticación solo principal (como la autenticación integrada de Windows o el nombre de usuario y la contraseña a través de la AD FS páginas web) .  Si el usuario no tiene verificación métodos configurados, Azure AD realizar el registro en línea en el que el usuario ve el mensaje "el administrador necesaria configurar esta cuenta para la comprobación de seguridad adicional", y el usuario puede seleccionar "Configurarlo ahora".
> Los usuarios que ya tienen al menos un método de verificación de MFA configurado aún se pedirá proporcionar MFA cuando visita la página proofup.

### <a name="recommended-deployment-topologies"></a>Topologías de implementación recomendados

#### <a name="azure-mfa-as-primary-authentication"></a>Azure AMF como autenticación principal
Hay un par de buenas razones para usar MFA Azure como autenticación principal con AD FS:
 - Para evitar las contraseñas de inicio de sesión en Azure AD, Office 365 y otras aplicaciones de AD FS
 - Para proteger la contraseña en función de inicio de sesión mediante la necesidad de un factor adicional como código de verificación antes de la contraseña

Si deseas usar MFA Azure como un método de autenticación principal en AD FS para lograr estos beneficios, probablemente también quieres mantener la capacidad de usar Azure AD condicional access como "true MFA" solicitando para factores adicionales en AD FS.

Ahora puede hacerlo mediante la configuración de dominio de Azure AD hacer MFA en locales (opción "SupportsMfa" $true).  En esta configuración, AD FS puede se te pida por Azure AD para realizar la autenticación adicional o "MFA true" para escenarios de acceso condicional que la requieren.  

Como se describió anteriormente, cualquier usuario de AD FS que no ha necesario instarlo (configurado MFA verificación información registrada) a través de una página de error personalizada de AD FS visitar aka.ms/mfasetup para configurar la información de comprobación, a continuación, volver a intentar el inicio de sesión de AD FS.  
Dado que Azure AMF como principal se considera un factor único, después de que los usuarios de la configuración inicial, deberás proporcionar un factor adicional para administrar o actualizar su información de comprobación en Azure AD, o para tener acceso a otros recursos que requieren MFA.


#### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure AMF como autenticación adicional a Office 365
Anteriormente, si desea realizar Azure AMF como un método de autenticación adicional en AD FS para Office 365 u otros usuarios de confianza, la mejor opción configurar Azure AD para compuestos MFA, en el que la autenticación principal se realiza en instalaciones en AD FS y MFA es tr iggered por Azure AD. Ahora, puedes usar Azure AMF como autenticación adicional en AD FS cuando la configuración de dominio SupportsMfa esté establecida en $True.  

Como se describió anteriormente, cualquier usuario de AD FS que no ha necesario instarlo (configurado MFA verificación información registrada) a través de una página de error personalizada de AD FS visitar aka.ms/mfasetup para configurar la información de comprobación, a continuación, volver a intentar el inicio de sesión de AD FS.  


## <a name="pre-requisites"></a>Requisitos previos  
Los siguientes requisitos previos son necesarios para usar MFA de Azure para la autenticación con AD FS:  
  
- Un [suscripción de Azure con Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Azure la autenticación multifactor](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  

>[!NOTE]   
> Azure AD y Azure MFA se incluyen en Azure AD Premium y Enterprise Mobility Suite (EMS).  Si tienes alguno de estos no es necesario suscripciones individuales.   
- Un entorno de Windows Server 2016 AD FS local.  
- El entorno local es [federados con Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Módulo de Windows Azure Active Directory para Windows PowerShell](https://go.microsoft.com/fwlink/p/?linkid=236297).  
- Permisos de administrador global en la instancia de Azure AD y configurarla con Azure AD PowerShell.  
- Credenciales de administrador de empresa para configurar el conjunto de AD FS MFA de Azure.  
  
  
## <a name="configure-the-ad-fs-servers"></a>Configurar los servidores de AD FS  
Para completar la configuración de Azure MFA de AD FS, deberás configurar cada servidor de AD FS siguiendo los pasos. 

>[!NOTE]
>Asegúrate de que estos pasos se realizan en **todos los** servidores de AD FS de la batería. Si tienes varios servidores de AD FS en su conjunto, puedes realizar la configuración necesaria de forma remota mediante Powershell de Azure AD.  
  
  
  
### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Paso 1: Generar un certificado para Azure MFA en cada servidor de AD FS usando el `New-AdfsAzureMfaTenantCertificate`cmdlet.   
Lo primero que tienes que hacer es generar un certificado de MFA de Azure usar.  Esto puede realizarse mediante PowerShell.  El certificado generado puede encontrarse en el almacén de certificados de equipos locales y está marcada con un nombre de sujeto que contiene la TenantID para el directorio de Azure AD.    
  
![AD FS y MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  
  
Ten en cuenta que TenantID es el nombre del directorio de Azure AD.  Usa el siguiente cmdlet de PowerShell para generar el nuevo certificado.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  
      
![AD FS y MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-azure-multi-factor-auth-client-spn"></a>Paso 2: Agregar las nuevas credenciales a SPN de cliente de autenticación de multifactor de Azure   
Para habilitar los servidores de AD FS para comunicarse con el cliente de autenticación multifactor de Azure, deberás agregar las credenciales para el SPN para el cliente de autenticación multifactor de Azure. Los certificados que se genera usando los `New-AdfsAzureMFaTenantCertificate`cmdlet servirá como estas credenciales. Haz lo siguiente mediante PowerShell para agregar las nuevas credenciales para el SPN de cliente de autenticación de multifactor de Azure.  

>[!NOTE]   
>Para completar este paso necesitas conectarte a la instancia de Azure AD con PowerShell usando Connect-MsolService.  Estos pasos se supone que ya se ha conectado mediante PowerShell.  Para obtener información, consulta [MsolService de Connect.](https://msdn.microsoft.com/library/dn194123.aspx)  
     
  
2. **Establece el certificado como la nueva credencial con el cliente de autenticación multifactor de Azure**  
    
    `New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64 `

>[!IMPORTANT]
>  Este comando debe ejecutarse en todos los servidores de AD FS de la batería.  MFA de Azure AD se producirá un error en los servidores que no tienen el certificado que se establece como la nueva credencial con el cliente de autenticación multifactor de Azure. 

>[!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 es el guid para el cliente de autenticación multifactor de Azure.  
  
## <a name="configure-the-ad-fs-farm"></a>Configure el conjunto de AD FS  
  
Después de completar la sección anterior en cada servidor de AD FS, tendrás que ejecutar el `Set-AdfsAzureMfaTenant`cmdlet.  
  
Este cmdlet se debe realizar una sola vez para una granja de AD FS.  Usar PowerShell para completar este paso.    
  
>[!NOTE]  
>Deberás reiniciar el servicio de AD FS en cada servidor de la batería antes de que estos cambios surtan efecto.  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  
      
![AD FS y MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
A continuación, verás que MFA de Azure está disponible como un método de autenticación principal de intranet y extranet uso.    
  
![AD FS y MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Renovar y administrar AD FS Azure MFA certificados
La siguiente guía detalla cómo administrar los certificados de MFA de Azure en los servidores de AD FS.
De manera predeterminada, cuando se configura AD FS con Azure MFA, los certificados que se generan mediante el cmdlet de PowerShell New-AdfsAzureMfaTenantCertificate son válidos durante 2 años.  Para determinar cómo cerrar para expiración los certificados son y, a continuación, para renovar e instalar los certificados nuevo, usa el siguiente procedimiento.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Evaluar la fecha de expiración del certificado de AD FS Azure MFA
En cada servidor de AD FS, en el equipo local Mi almacén, habrá un certificado autofirmado con "OU = Microsoft AD FS Azure MFA" en el emisor y el asunto.  Este es el certificado de MFA de Azure.  Comprueba el período de validez del certificado en cada servidor de AD FS para determinar la fecha de expiración.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Crear nuevo AD FS Azure MFA certificado en cada servidor de AD FS
Si el período de validez de los certificados se está acercando a su final, iniciar el proceso de renovación al generar un nuevo certificado MFA de Azure en cada servidor de AD FS. En una ventana de comandos de powershell, generar un nuevo certificado en cada servidor de AD FS mediante el cmdlet siguiente:

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

Como resultado de este cmdlet, se generará un nuevo certificado válido de 2 días en el futuro, 2 días + 2 años.  Las operaciones de AD FS y MFA de Azure no se verán afectadas por este cmdlet o el nuevo certificado. (Nota: el retraso de día 2 es intencionado y proporciona tiempo para ejecutar los siguientes pasos para configurar el nuevo certificado en el inquilino antes de AD FS inicia el uso de Azure MFA.)


### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurar cada nuevo certificado de AD FS Azure MFA en el inquilino de Azure AD
Con el módulo de PowerShell de Azure AD, para cada nuevo certificado (en cada servidor de AD FS), actualiza la configuración de inquilino de Azure AD como sigue (Nota: en primer lugar debes conectarte al inquilino con Connect-MsolService para ejecutar los comandos siguientes).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $certbase64
```
    Where $certbase64 is the new certificate.  The base64 encoded certificate can be obtained by exporting the certificate (without the private key) as a DER encoded file and opening in Notepad.exe, then copy/pasting to the PSH session and assigning to the variable $certbase64

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Comprueba que los nuevos certificados se usará para MFA de Azure
Una vez los nuevos certificados sean válidos, AD FS seleccionarlos y empezar a usar cada certificado respectivo MFA de Azure en pocas horas al día.  Una vez que esto ocurre, en cada servidor, verás un evento iniciado sesión en el registro de eventos de administrador de AD FS con la siguiente información: nombre del registro: AD FS/Admin origen: AD FS fecha: 27/2/2018 7:33:31 P.M. identificador de evento: 547 Task Category: None nivel: palabras clave de información : AD FS usuario: equipo DOMAIN\adfssvc: ADFS.domain.contoso.com Descripción: se ha renovado el certificado de inquilino de Azure MFA.  

TenantId: contoso.onmicrosoft.com. Huella digital antiguo: 7CC103D60967318A11D8C51C289EF85214D9FC63. Fecha de expiración antiguo: 15/9/2019 9:43:17 PM. Huella digital nuevo: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF. Nueva fecha de expiración: 2/27 de 2020 2:16:07 AM.

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personalizar la página web de AD FS para guiar a los usuarios para registrar los métodos de comprobación de MFA

Usa los siguientes ejemplos personalizar las páginas web de AD FS para los usuarios que no hayan compatible tecnologías aún arriba (información de comprobación de MFA configurado).

### <a name="find-the-error"></a>Encontrar el error
En primer lugar, hay un par de AD FS devolverá en el caso en el que el usuario no tiene información de comprobación de mensajes de error diferente.
Si usas Azure AMF como autenticación principal, el usuario no proofed verá una página de error de AD FS que contiene los siguientes mensajes de:
```
    <div id="errorArea"> 
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred 
        </div> 
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information. 
        </div>
```
Cuando se intenta Azure AD como autenticación adicional, el usuario no proofed verá una página de error de AD FS que contiene los siguientes mensajes de:
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

### <a name="catch-the-error-and-update-the-page-text"></a>Capturar el error y actualizar el texto de la página
Capturar el error y mostrar al usuario personalizado guía es una cuestión de anexar javascript hasta el final del archivo onload.js que forma parte de la AD FS web tema para buscar (1) para las cadenas de error de identificación y (2) proporcionar personalizada contenido Web.  (Para obtener instrucciones en general acerca de cómo personalizar el archivo onload.js, consulta el artículo [aquí](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/advanced-customization-of-ad-fs-sign-in-pages).)

