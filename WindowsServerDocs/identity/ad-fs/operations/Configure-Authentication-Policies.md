---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configuración de directivas de autenticación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ef38b0280a5753b0995e85d0809de6b632fa3afc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358120"
---
# <a name="configure-authentication-policies"></a>Configuración de directivas de autenticación

En AD FS, en Windows Server 2012 R2, el control de acceso y el mecanismo de autenticación se han mejorado con varios factores que incluyen los datos de usuario, dispositivo, ubicación y autenticación. Estas mejoras le permiten, ya sea a través de la interfaz de usuario o a través de Windows PowerShell, para administrar el riesgo de conceder permisos de acceso\-a AD FS\-aplicaciones protegidas mediante el control de acceso multifactor y multi\-factor de autenticación que se basa en la identidad del usuario o la pertenencia a grupos, la ubicación\-de red, los datos del dispositivo Unidos\-al área \(de trabajo y el estado de autenticación cuando MFAdeautenticaciónmultifactor\) se ha realizado.  

Para obtener más información acerca de MFA\-y el control de acceso \(multifactor en servicios de Federación de Active Directory (AD FS) AD FS\) en Windows Server 2012 R2, vea los temas siguientes:  


-   [Unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Administración de riesgos con control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurar directivas de autenticación mediante el complemento\-de administración de AD FS  
El requisito mínimo para realizar estos procedimientos es la pertenencia al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   

En AD FS, en Windows Server 2012 R2, puede especificar una directiva de autenticación en un ámbito global que se aplique a todas las aplicaciones y servicios que están protegidos por AD FS. También puede establecer directivas de autenticación para aplicaciones y servicios específicos que se basan en las relaciones de confianza de entidades y están protegidos por AD FS. La especificación de una directiva de autenticación para una aplicación determinada por relación de confianza para usuario autenticado no invalida la Directiva de autenticación global. Si una directiva de autenticación global o por relación de confianza para usuario autenticado requiere MFA, se desencadena MFA cuando el usuario intenta autenticarse en esta relación de confianza para usuario autenticado. La Directiva de autenticación global es una reserva para las relaciones de confianza para usuario autenticado para aplicaciones y servicios que no tienen una directiva de autenticación configurada concreta. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Para configurar la autenticación principal globalmente en Windows Server 2012 R2 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En AD FS complemento\-, haga clic en **directivas de autenticación**.  

3.  En la sección **autenticación principal** , haga clic en **Editar** junto a **configuración global**. También puede hacer clic\-con el botón derecho en **directivas de autenticación**y seleccionar **Editar autenticación principal global**, o bien, en el panel **acciones** , seleccione **Editar autenticación principal global**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy1.png)

4.  En la ventana **Editar Directiva de autenticación global** , en la pestaña **principal** , puede configurar las siguientes opciones como parte de la Directiva de autenticación global:  

    -   Métodos de autenticación que se usarán para la autenticación principal. Puede seleccionar métodos de autenticación disponibles en la **extranet** y la **intranet**.  

    -   Autenticación de dispositivos mediante la casilla **Habilitar autenticación de dispositivo** . Para obtener más información, consulte [unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Para configurar la autenticación principal por relación de confianza para usuario autenticado  

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En AD FS complemento\-, haga clic en **directivas**\\de autenticación**por relación de confianza para usuario autenticado**y, a continuación, haga clic en la relación de usuario de confianza para la que desea configurar las directivas de autenticación.  

3.  Haga clic\-con el botón derecho en la relación de usuario de confianza para la que desea configurar las directivas de autenticación y, después, seleccione **Editar autenticación principal personalizada**, o bien, en el panel **acciones** , seleccione **Editar principal personalizada. Autenticación**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  En la ventana **Editar Directiva de autenticación para <\_nombre\_de\_la relación de confianza para usuario autenticado >** , en la pestaña **principal** , puede configurar la opción siguiente como parte de la relación de confianza para usuario **autenticado** . Directiva de autenticación:  

    -   Si los usuarios deben proporcionar sus credenciales cada vez que inicie\-sesión a través de los **usuarios deben proporcionar sus credenciales cada vez que\-inicie sesión** .  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Para configurar la autenticación multifactor globalmente  

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En AD FS complemento\-, haga clic en **directivas de autenticación**.  

3.  En la **sección\-multi-factor Authentication** , haga clic en **Editar** junto a **configuración global**. También puede hacer clic\-con el botón derecho en **directivas de autenticación**y **\-seleccionar Editar autenticación multifactor global**, o bien, **\-en el panel acciones, seleccione Editar autenticación multifactor global.** .  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  En la ventana **Editar Directiva de autenticación global** , en la pestaña **\-multi-factor** , puede configurar las siguientes opciones\-como parte de la Directiva de autenticación multifactor global:  

    -   Configuración o condiciones para MFA a través de las opciones disponibles en las secciones **grupos de\/usuarios**, **dispositivos**y **ubicaciones** .  

    -   Para habilitar MFA para cualquiera de estas opciones, debe seleccionar al menos un método de autenticación adicional. La **autenticación de certificado** es la opción disponible de forma predeterminada. También puede configurar otros métodos de autenticación adicionales personalizados, por ejemplo, la autenticación activa de Windows Azure. Para obtener más información, [consulte Guía de tutorial: Administrar los riesgos con Multi-Factor Authentication adicionales para las](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)aplicaciones confidenciales.  

> [!WARNING]  
> Solo puede configurar métodos de autenticación adicionales de forma global.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Para configurar multi\--factor Authentication por relación de confianza para usuario autenticado  

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En AD FS complemento\-, haga clic en **directivas**\\de autenticación**por relación de confianza para usuario autenticado**y, a continuación, haga clic en la relación de usuario de confianza para la que desea configurar MFA.  

3.  Haga clic\-con el botón derecho en la relación de usuario de confianza para la que desea configurar MFA y, a continuación, seleccione **\-editar autenticación multifactor personalizada**o, en el panel **acciones** , seleccione **\- editar múltiple personalizado. Autenticación de factor**.  

4.  En la ventana **Editar Directiva de autenticación para <\_nombre\_de\_la relación de confianza para usuario autenticado >** , en la pestaña **multi\--factor** , puede configurar las siguientes opciones como parte de por\-Directiva de autenticación de relación de confianza para usuario autenticado:  

    -   Configuración o condiciones para MFA a través de las opciones disponibles en las secciones **grupos de\/usuarios**, **dispositivos**y **ubicaciones** .  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurar directivas de autenticación mediante Windows PowerShell  
Windows PowerShell ofrece una mayor flexibilidad a la hora de usar varios factores de control de acceso y el mecanismo de autenticación que están disponibles en AD FS en Windows Server 2012 R2 para configurar las directivas de autenticación y las reglas de autorización necesarias para implemente un verdadero acceso condicional para \-los recursos protegidos AD FS.  

La pertenencia al grupo administradores, o equivalente, en el equipo local es el requisito mínimo para completar estos procedimientos.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en \( [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Para configurar un método de autenticación adicional a través de Windows PowerShell  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
`Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `
~~~


> [!WARNING]  
> Para comprobar si este comando se ejecutó correctamente, puede ejecutar el comando `Get-AdfsGlobalAuthenticationPolicy` .  

### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Para configurar MFA por\-relación de confianza para usuario autenticado que se basa en los datos de pertenencia a grupos de un usuario  

1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando:  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!WARNING]  
> Asegúrese de reemplazar *\_< relación de\_confianza para usuario autenticado >* por el nombre de la relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  


~~~
$MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'”, Value =~ ‘“^(?i) <group_SID>$'”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn'”);” 

Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
~~~


> [!NOTE]  
> Asegúrese de reemplazar < SID\_de grupo > por el valor del \(SID\) del identificador de seguridad del grupo \(de\) Active Directory ad.  

### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Para configurar MFA globalmente en función de los datos de pertenencia a grupos de los usuarios  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value == ‘"group_SID'"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< SID\_de grupo >* por el valor del SID del grupo de ad.  

### <a name="to-configure-mfa-globally-based-on-users-location"></a>Para configurar MFA globalmente en función de la ubicación del usuario  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == ‘"true_or_false'"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~



> [!NOTE]  
> Asegúrese de reemplazar *< >\_true\_o false* por `true` o `false`. El valor depende de la condición de regla específica que se basa en si la solicitud de acceso procede de la extranet o de la intranet.  

### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Para configurar MFA globalmente según los datos del dispositivo del usuario  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
$MfaClaimRule = "c:[Type == ‘" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == ‘"true_or_false"']  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< >\_true\_o false* por `true` o `false`. El valor depende de la condición de regla específica que se basa en si el dispositivo está\-unido al área de trabajo o no.  

### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Para configurar MFA globalmente si la solicitud de acceso procede de la extranet y de un\-dispositivo\-que no está unido al área de trabajo  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
`Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
~~~


> [!NOTE]  
> Asegúrese de reemplazar ambas instancias de *< >\_true\_o false* por `true` o `false`, que depende de las condiciones de regla específicas. Las condiciones de la regla se basan en si el dispositivo\-está unido al área de trabajo o no, y si la solicitud de acceso procede de la extranet o la intranet.  

### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Para configurar MFA globalmente si el acceso procede de un usuario de extranet que pertenece a un grupo determinado  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
~~~

> [!NOTE]  
> Asegúrese de reemplazar *< SID\_de grupo >* por el valor del SID de grupo y *<\_>\_true o false* por `true` o `false`, que depende de la condición de regla específica que se basa en si la solicitud de acceso procede de la extranet o la intranet.  

### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Para conceder acceso a una aplicación basada en datos de usuario a través de Windows PowerShell  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Asegúrese de reemplazar *< > de\_\_relación de confianza para usuario autenticado* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  

   ```  

     $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
   Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
   ```  

> [!NOTE]  
> > Asegúrese de reemplazar *< SID\_de grupo >* por el valor del SID del grupo de ad.  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Para conceder acceso a una aplicación protegida por AD FS solo si la identidad de este usuario se validó con MFA  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< > de\_\_relación de confianza para usuario autenticado* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  

   ```  
   $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
   @RuleName = `"PermitAccessWithMFA`"  
   c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim'");"  

   ```  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Para conceder acceso a una aplicación protegida por AD FS solo si la solicitud de acceso procede de un dispositivo\-unido al área de trabajo registrado para el usuario  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Asegúrese de reemplazar *< > de\_\_relación de confianza para usuario autenticado* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
~~~



### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acceso a una aplicación protegida por AD FS solo si la solicitud de acceso procede de un dispositivo\-unido al área de trabajo registrado a un usuario cuya identidad se ha validado con MFA  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< > de\_\_relación de confianza para usuario autenticado* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  

   ```  
   $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
   @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
   c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
   c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

   ```  

### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acceso a Extranet a una aplicación protegida por AD FS solo si la solicitud de acceso procede de un usuario cuya identidad se ha validado con MFA  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< > de\_\_relación de confianza para usuario autenticado* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"RequireMFAForExtranetAccess`"  
c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
~~~

## <a name="additional-references"></a>Referencias adicionales  

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
