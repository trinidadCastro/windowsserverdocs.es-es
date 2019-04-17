---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: "Configurar las directivas de autenticación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7faffb7ccbb4b0ea3c65329d18f915d7dafcd46f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-authentication-policies"></a>Configurar las directivas de autenticación

>Se aplica a: Windows Server 2012 R2

En AD FS, en Windows Server 2012 R2, ambos control de acceso y el mecanismo de autenticación se han mejorado con varios factores que incluyen los datos de usuario, dispositivo, la ubicación y autenticación. Estas mejoras permiten, mediante la interfaz de usuario o mediante Windows PowerShell, para administrar el riesgo de conceder permisos de acceso a aplicaciones seguras FS\ anuncios a través de control de acceso de factor de multi\ y autenticación de factor de multi\ que se basan en los miembros del grupo o la identidad de usuario, la ubicación de red, los datos de dispositivo que está unido a workplace\, y el estado de autenticación cuando la autenticación de factor de multi\ \(MFA\) fue realizado.  
  
Para obtener más información acerca del control de acceso MFA y el factor de multi\ en los servicios de federación de Active Directory \(AD FS\) en Windows Server 2012 R2, consulta los siguientes temas:  


-   [Unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación entre las aplicaciones de empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Administrar el riesgo con Control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurar las directivas de autenticación a través de la administración de AD FS snap\ en  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar estos procedimientos.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
En AD FS, en Windows Server 2012 R2, puedes especificar una directiva de autenticación en un ámbito global que se aplica a todas las aplicaciones y servicios que están protegidos por AD FS. También puedes establecer directivas de autenticación para determinadas aplicaciones y servicios que dependen de confianzas de terceros y se protegen mediante AD FS. Especificar una directiva de autenticación para una aplicación determinada por confiar confianza de terceros no reemplaza la directiva de autenticación global. Si globales o por usar la directiva de autenticación requiere MFA, MFA de confianza de terceros se desencadena cuando el usuario intente autenticarse en esta confianza de terceros de confianza. La directiva de autenticación global es una reserva para usuarios de confianza confianzas de terceros para aplicaciones y servicios que no tienen una directiva de autenticación configurado específico. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Para configurar la autenticación principal globalmente en Windows Server 2012 R2 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En AD FS en snap\, haz clic en **directivas de autenticación**.  
  
3.  En la **autenticación principal** sección, haz clic en **editar** junto a **configuración Global**. También puedes right\ clic **directivas de autenticación**y selecciona **editar Global autenticación principal**, o bien, en la **acciones** panel, selecciona **editar Global autenticación principal**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  En la **Editar directiva de autenticación Global** ventana, en la **principal** ficha, puedes configurar la siguiente configuración como parte de la directiva de autenticación global:  
  
    -   Métodos de autenticación que se usará para la autenticación principal. Puedes seleccionar los métodos de autenticación disponibles en la **Extranet** y **Intranet**.  
  
    -   Autenticación de dispositivo a través de la **habilitar la autenticación de dispositivo** casilla de verificación. Para obtener más información, consulta [unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Para configurar la autenticación principal por confiar confianza de terceros  
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En AD FS en snap\, haz clic en **directivas de autenticación**\\**por terceros confianza de confiar**y, a continuación, haz clic en la confianza de terceros confianza para el que quieres configurar las directivas de autenticación.  
  
3.  Right\-haga clic en el usuario de confianza de confianza para el que quieres configurar las directivas de autenticación y, a continuación, seleccione **autenticación principal personalizada editar**, o bien, en la **acciones** panel, selecciona **autenticación principal personalizada editar**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  En la **Editar directiva de autenticación para < relying\_party\_trust\_name >** ventana, en la **principal** ficha, puedes configurar la siguiente configuración como parte de la **por confiar terceros confianza** directiva de autenticación:  
  
    -   Si es necesario para proporcionar sus credenciales cada vez en sign\ en los usuarios a través de la **es necesario para proporcionar sus credenciales en sign\ en cada vez que los usuarios** casilla de verificación.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Para configurar la autenticación multifactor de todo el mundo  
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En AD FS en snap\, haz clic en **directivas de autenticación**.  
  
3.  En la **autenticación de factor de Multi\** sección, haz clic en **editar** junto a **configuración Global**. También puedes right\ clic **directivas de autenticación**y selecciona **editar Global de Multi\ factor autenticación**, o bien, en la **acciones** panel, selecciona **editar Global de Multi\ factor autenticación**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  En la **Editar directiva de autenticación Global** ventana, en la **Multi\ factor** ficha, puedes configurar la siguiente configuración como parte de la directiva de autenticación de factor de multi\ global:  
  
    -   Configuración o condiciones de MFA a través de las opciones disponibles en la **Users\ o grupos**, **dispositivos**, y **ubicaciones** secciones.  
  
    -   Para habilitar AMF para cualquiera de estas opciones de configuración, debes seleccionar al menos un método de autenticación adicional. **Autenticación de certificado** es la opción disponible de forma predeterminada. También puedes configurar otros métodos de autenticación adicionales personalizados, por ejemplo, Windows Azure Active la autenticación. Para obtener más información, consulta [guía paso a paso: administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
> [!WARNING]  
> Sólo se pueden configurar los métodos de autenticación adicional de todo el mundo.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Para configurar la autenticación de factor de multi\ por confiar confianza de terceros  
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En AD FS en snap\, haz clic en **directivas de autenticación**\\**por terceros confianza de confiar**y, a continuación, haz clic en la confianza de terceros confianza para el que desea configurar MFA.  
  
3.  Right\-haga clic en el usuario de confianza de confianza para el que desea configurar MFA y, a continuación, seleccione **autenticación de factor Multi\ editar personalizada**, o bien, en la **acciones** panel, selecciona **autenticación de factor Multi\ editar personalizada**.  
  
4.  En la **Editar directiva de autenticación para < relying\_party\_trust\_name >** ventana, en la **Multi\ factor** ficha, puedes configurar la siguiente configuración como parte de la parte confiar per\ confiar en directiva de autenticación:  
  
    -   Configuración o condiciones de MFA a través de las opciones disponibles en la **Users\ o grupos**, **dispositivos**, y **ubicaciones** secciones.  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurar las directivas de autenticación a través de Windows PowerShell  
Windows PowerShell permite mayor flexibilidad en el uso de varios factores de control de acceso y el mecanismo de autenticación que están disponibles en AD FS en Windows Server 2012 R2 para configurar las directivas de autenticación y autorización reglas que son necesarios para implementar true acceso condicional para los recursos de AD FS \-secured.  
  
Pertenencia al grupo Administradores o equivalente, en el equipo local es el requisito mínimo para completar estos procedimientos.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Para configurar un método de autenticación adicional a través de Windows PowerShell  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> Para comprobar que este comando se ejecutó correctamente, puedes ejecutar el `Get-AdfsGlobalAuthenticationPolicy` comando.  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Para configurar confiar per\ MFA confianza de terceros que se basa en los datos de pertenencia al grupo de un usuario  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando:  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> Asegúrate de que se reemplace *< relying\_party\_trust >* con el nombre de tu confianza confianza de terceros.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando.  
  
 
    $MfaClaimRule = "c: [tipo == '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", valor = ~ '" ^(?i) < group_SID >$ ' "] = > problema (tipo = '" https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor '" https://schemas.microsoft.com/claims/multipleauthn'");" 
    
    Conjunto AdfsRelyingPartyTrust – TargetRelyingParty $rp: AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> Asegúrate de que se reemplace < group\_SID > con el valor del identificador de seguridad \(SID\) de tu grupo \(AD\) Active Directory.  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Para configurar MFA globalmente en función de datos de pertenencia al grupo de usuarios  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  

    $MfaClaimRule = "c: [tipo == '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", valor == '" group_SID'"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
      
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Asegúrate de que se reemplace *< group\_SID >* con el valor del SID de tu grupo AD.  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>Para configurar MFA globalmente en función de la ubicación del usuario  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  
 
    $MfaClaimRule = "c: [tipo == '" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", valor == '" true_or_false'"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> Asegúrate de que se reemplace *< true\_or\_false >* con cualquiera `true` o `false`. El valor depende de la condición de regla específica que se basa en si la solicitud de acceso procede de la extranet o de la intranet.  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Para configurar MFA globalmente en función de datos de dispositivo del usuario  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  

    $MfaClaimRule = "c: [tipo == '" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", valor == '" true_or_false"']  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Asegúrate de que se reemplace *< true\_or\_false >* con cualquiera `true` o `false`. El valor depende de la condición de regla específica que se basa en si el dispositivo está unido a workplace\ o no.  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Para configurar todo el mundo MFA si la solicitud de acceso que se incluye en la extranet y desde un dispositivo unido a non\-workplace\  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> Asegúrate de que se reemplace ambas instancias de *< true\_or\_false >* con cualquiera `true` o `false`, que depende de las condiciones de regla específica. Las condiciones de regla se basan en si el dispositivo esté unido workplace\ o no y si la solicitud de acceso proviene la extranet o intranet.  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Para configurar todo el mundo MFA si acceso procede de un usuario extranet que pertenece a un determinado grupo  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  

    Set-AdfsAdditionalAuthenticationRule "c: [tipo == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", valor == `"group_SID`"] & & c2: [tipo == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", valor == `"true_or_false`"] = > problema (tipo = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", valor ='"https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> Asegúrate de que se reemplace *< group\_SID >* con el valor del SID del grupo y *< true\_or\_false >* con cualquiera `true` o `false`, que depende de la condición de regla específica que se basa en si la solicitud de acceso proviene la extranet o intranet.  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Para conceder acceso a una aplicación basada en datos de usuario a través de Windows PowerShell  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Asegúrate de que se reemplace *< relying\_party\_trust >* con el valor de tu confianza confianza de terceros.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando.  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > Asegúrate de que se reemplace *< group\_SID >* con el valor del SID de tu grupo AD.  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Para conceder el acceso a una aplicación que está protegida por solo si de AD FS la identidad del usuario se ha validado con MFA  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Asegúrate de que se reemplace *< relying\_party\_trust >* con el valor de tu confianza confianza de terceros.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando.  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Para conceder acceso a una aplicación que está protegida por AD FS solo si la solicitud de acceso proviene de un dispositivo unido workplace\ que esté registrado para el usuario  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Asegúrate de que se reemplace *< relying\_party\_trust >* con el valor de tu confianza confianza de terceros.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c: [tipo == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", valor = ~ `"^(?i)true$`"] = > problema (tipo = `"https://schemas.microsoft.com/authorization/claims/permit`", valor = `"PermitUsersWithClaim`");  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acceso a una aplicación que está protegida por AD FS solo si la solicitud de acceso proviene de un dispositivo unido workplace\ que esté registrado a un usuario cuya identidad se ha validado con MFA  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Asegúrate de que se reemplace *< relying\_party\_trust >* con el valor de tu confianza confianza de terceros.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando.  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acceso extranet a una aplicación protegida por AD FS solo si la solicitud de acceso proviene de un usuario cuya identidad se ha validado con MFA  
  
1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> Asegúrate de que se reemplace *< relying\_party\_trust >* con el valor de tu confianza confianza de terceros.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    C1: [tipo == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", valor = ~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] & &  
    C2: [tipo == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", valor = ~ `"^(?i)false$`"] = > problema (tipo = `"https://schemas.microsoft.com/authorization/claims/permit`", valor = `"PermitUsersWithClaim`"); "  

## <a name="additional-references"></a>Referencias adicionales  

[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md)
