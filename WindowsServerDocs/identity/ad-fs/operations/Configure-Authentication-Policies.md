---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configuración de directivas de autenticación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9345f995af2f256dddcbcbd7d05c4bf6170b563e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189860"
---
# <a name="configure-authentication-policies"></a>Configuración de directivas de autenticación

En AD FS en Windows Server 2012 R2, ambos control de acceso y se han mejorado el mecanismo de autenticación con varios factores que incluyen datos de usuario, dispositivo, ubicación y autenticación. Estas mejoras permiten a usted, a través de la interfaz de usuario o a través de Windows PowerShell, para administrar el riesgo de conceder permisos de acceso a AD FS\-protege las aplicaciones a través de múltiples\-factorizar el control de acceso y de varios\-autenticación multifactor que se basan en los miembros del grupo o la identidad de usuario, la ubicación de red, datos del dispositivo al área de trabajo\-Unido, y el estado de la autenticación cuando varios\-autenticación multifactor \(MFA\) se realizó.  
  
Para obtener más información acerca de MFA y de varios\-factorizar el control de acceso en Active Directory Federation Services \(AD FS\) en Windows Server 2012 R2, vea los temas siguientes:  


-   [Unirse a un área de trabajo desde cualquier dispositivo para SSO y sin interrupciones de segundo Factor de autenticación a través de las aplicaciones de empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Administración de riesgos con control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurar directivas de autenticación mediante el complemento Administración de AD FS\-en  
El requisito mínimo para realizar estos procedimientos es la pertenencia al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
En AD FS en Windows Server 2012 R2, puede especificar una directiva de autenticación en un ámbito global que se aplica a todas las aplicaciones y servicios que están protegidos por AD FS. También puede establecer directivas de autenticación para aplicaciones específicas y los servicios que dependen de las relaciones y están protegidos por AD FS. Especificar una directiva de autenticación para una aplicación concreta por usuarios de confianza de confianza no invalida la directiva de autenticación global. Si global o por usuarios de confianza de confianza directiva de autenticación requiere MFA, MFA se desencadena cuando el usuario intente autenticarse a esta relación de confianza. La directiva de autenticación global es una reserva para relaciones de confianza para aplicaciones y servicios que no tienen una directiva de autenticación configurado específico. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Para configurar la autenticación principal globalmente en Windows Server 2012 R2 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En AD FS ajustar\-en, haga clic en **las directivas de autenticación**.  
  
3.  En el **autenticación principal** sección, haga clic en **editar** junto a **configuración Global**. También puede secundario\-haga clic en **las directivas de autenticación**y seleccione **Editar autenticación principal Global**, o bien, en el **acciones** panel, seleccione  **Editar autenticación principal Global**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  En el **Editar directiva de autenticación Global** ventana, en el **principal** ficha, puede configurar las opciones siguientes como parte de la directiva de autenticación global:  
  
    -   Métodos de autenticación que se usará para la autenticación principal. Puede seleccionar los métodos de autenticación disponibles en el **Extranet** y **Intranet**.  
  
    -   Autenticación de dispositivos a través de la **Habilitar autenticación de dispositivo** casilla de verificación. Para obtener más información, consulte [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Para configurar la autenticación principal por usuarios de confianza de confianza  
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En AD FS ajustar\-en, haga clic en **las directivas de autenticación**\\**por usuario de confianza**y, a continuación, haga clic en la relación de confianza para el que desea configurar la autenticación directivas.  
  
3.  Directamente\-haga clic en la relación de confianza para el que desea configurar las directivas de autenticación y, a continuación, seleccione **Editar autenticación principal personalizada**, o bien, en el **acciones** panel, Seleccione **Editar autenticación principal personalizada**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  En el **Editar directiva de autenticación para < usuario de confianza\_entidad\_confianza\_nombre >** ventana, en el **principal** ficha, puede configurar la siguiente configuración como parte de la **por confianza** directiva de autenticación:  
  
    -   Si los usuarios deben proporcionar sus credenciales cada vez que inician sesión\-en a través de la **los usuarios deben proporcionar sus credenciales cada vez que inician sesión\-en** casilla de verificación.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Para configurar la autenticación multifactor globalmente  
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En AD FS ajustar\-en, haga clic en **las directivas de autenticación**.  
  
3.  En el **Multi\-autenticación fases** sección, haga clic en **editar** junto a **configuración Global**. También puede secundario\-haga clic en **las directivas de autenticación**y seleccione **editar varios Global\-factor de autenticación**, o bien, en el **acciones**panel, seleccione **editar varios Global\-autenticación fases**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  En el **Editar directiva de autenticación Global** ventana, en el **Multi\-factor** ficha, puede configurar las opciones siguientes como parte de la múltiple global\-factor Directiva de autenticación:  
  
    -   Configuración o las condiciones para MFA a través de las opciones disponibles bajo la **usuarios\/grupos**, **dispositivos**, y **ubicaciones** secciones.  
  
    -   Para habilitar MFA para cualquiera de estas configuraciones, debe seleccionar al menos un método de autenticación adicional. **Un certificado de autenticación** es la opción disponible de forma predeterminada. También puede configurar otros métodos de autenticación adicional personalizado, por ejemplo, Windows Azure Active Authentication. Para obtener más información, consulte [guía paso a paso: Administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
> [!WARNING]  
> Solo se pueden configurar métodos de autenticación adicionales globalmente.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Para configurar múltiples\-autenticación multifactor por usuario de confianza de confianza  
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En AD FS ajustar\-en, haga clic en **las directivas de autenticación**\\**por usuario de confianza**y, a continuación, haga clic en la relación de confianza para el que desea configurar MFA.  
  
3.  Directamente\-haga clic en la relación de confianza para el que desea configurar MFA y, a continuación, seleccione **editar personalizado Multi\-autenticación fases**, o bien, en el **acciones** panel, Seleccione **editar personalizado Multi\-autenticación fases**.  
  
4.  En el **Editar directiva de autenticación para < usuario de confianza\_entidad\_confianza\_nombre >** ventana, en el **Multi\-factor** ficha, puede Configure las siguientes opciones como parte de la por\-directiva usuario de autenticación de confianza de confianza:  
  
    -   Configuración o las condiciones para MFA a través de las opciones disponibles bajo la **usuarios\/grupos**, **dispositivos**, y **ubicaciones** secciones.  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurar directivas de autenticación a través de Windows PowerShell  
Windows PowerShell permite una mayor flexibilidad en el uso de varios factores de control de acceso y el mecanismo de autenticación que están disponibles en AD FS en Windows Server 2012 R2 para configurar las directivas de autenticación y autorización de reglas que son necesarios para implementar el acceso condicional es true para AD FS \-los recursos protegidos.  
  
Pertenencia al grupo Administradores o equivalente, en el equipo local es el requisito mínimo para completar estos procedimientos.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Para configurar un método de autenticación adicional a través de Windows PowerShell  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> Para comprobar si este comando se ejecutó correctamente, puede ejecutar el comando `Get-AdfsGlobalAuthenticationPolicy` .  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Configuración de MFA por\-de confianza que se basa en datos de pertenencia a grupos del usuario  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando:  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> Asegúrese de reemplazar *< confiar\_entidad\_confianza >* con el nombre de la relación de confianza para usuario autenticado.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  
  
 
    $MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’”, Value =~ ‘“^(?i) <group_SID>$’”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn’”);” 
    
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> Asegúrese de reemplazar < grupo\_SID > con el valor del identificador de seguridad \(SID\) de Active Directory \(AD\) grupo.  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Para configurar MFA globalmente basándose en los datos de pertenencia de grupo de usuarios  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  

    $MfaClaimRule = "c: [tipo ==" " https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", valor == ' "group_SID'"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
      
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Asegúrese de reemplazar *< grupo\_SID >* con el valor del SID del grupo de AD.  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>Para configurar MFA globalmente en función de la ubicación del usuario  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  
 
    $MfaClaimRule = "c: [tipo ==" " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", valor == ' "true_or_false'"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> Asegúrese de reemplazar *< true\_o\_false >* con cualquiera `true` o `false`. El valor depende de la condición de regla concreta que se basa en si la solicitud de acceso procede de la extranet o en la intranet.  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Para configurar MFA globalmente basándose en datos del dispositivo del usuario  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  

    $MfaClaimRule = "c: [tipo ==" " https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", valor == '"true_or_false" ']  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Asegúrese de reemplazar *< true\_o\_false >* con cualquiera `true` o `false`. El valor depende de la condición de regla concreta que se basa en si el dispositivo es el área de trabajo\-Unido o no.  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Configuración global de MFA si la solicitud de acceso procede de la extranet y de un no\-workplace\-dispositivo unido a  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> Asegúrese de reemplazar las dos instancias de *< true\_o\_false >* con cualquiera `true` o `false`, que depende de las condiciones de regla concreta. Las condiciones de reglas se basan en si el dispositivo es el área de trabajo\-Unido o no y si la solicitud de acceso proviene de la intranet o de la intranet.  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Para configurar MFA globalmente si acceso proviene de un usuario de extranet que pertenece a un grupo concreto  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  

    Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> Asegúrese de reemplazar *< grupo\_SID >* con el valor del SID de grupo y *< true\_o\_false >* con cualquiera `true` o `false`, que depende de la condición de regla concreta que se basa en si la solicitud de acceso proviene de la intranet o de la intranet.  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Para conceder acceso a una aplicación basada en los datos de usuario mediante Windows PowerShell  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Asegúrese de reemplazar *< confiar\_entidad\_confianza >* con el valor de la relación de confianza para usuario autenticado.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > Asegúrese de reemplazar *< grupo\_SID >* con el valor del SID del grupo de AD.  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Para conceder acceso a una aplicación que solo si de AD FS se protege la identidad del usuario se ha validado con MFA  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Asegúrese de reemplazar *< confiar\_entidad\_confianza >* con el valor de la relación de confianza para usuario autenticado.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Para conceder acceso a una aplicación que solo si de AD FS se protege el acceso de solicitud proviene de un área de trabajo\-dispositivo unido al que está registrado en el usuario  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Asegúrese de reemplazar *< confiar\_entidad\_confianza >* con el valor de la relación de confianza para usuario autenticado.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acceso a una aplicación que solo si de AD FS se protege el acceso de solicitud proviene de un área de trabajo\-dispositivo unido al que está registrado para un usuario cuya identidad se ha validado con MFA  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Asegúrese de reemplazar *< confiar\_entidad\_confianza >* con el valor de la relación de confianza para usuario autenticado.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acceso a extranet para una aplicación protegida por AD FS solo si la solicitud de acceso procede de un usuario cuya identidad se ha validado con MFA  
  
1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> Asegúrese de reemplazar *< confiar\_entidad\_confianza >* con el valor de la relación de confianza para usuario autenticado.  
  
2.  En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

## <a name="additional-references"></a>Referencias adicionales  

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
