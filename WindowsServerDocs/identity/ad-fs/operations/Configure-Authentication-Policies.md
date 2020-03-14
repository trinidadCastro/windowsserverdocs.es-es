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
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323097"
---
# <a name="configure-authentication-policies"></a>Configuración de directivas de autenticación

En AD FS, en Windows Server 2012 R2, el control de acceso y el mecanismo de autenticación se han mejorado con varios factores que incluyen los datos de usuario, dispositivo, ubicación y autenticación. Estas mejoras le permiten, ya sea a través de la interfaz de usuario o a través de Windows PowerShell, para administrar el riesgo de conceder permisos de acceso a AD FS\-aplicaciones protegidas mediante el control de acceso multi\-factor y\-la autenticación multi\-factor que se basan en la identidad del usuario o la pertenencia a grupos, la ubicación de red, los datos del dispositivo que está en el área de\-\(\)  

Para obtener más información acerca de MFA y el control de acceso multi\-factor en Servicios de federación de Active Directory (AD FS) \(AD FS\) en Windows Server 2012 R2, vea los temas siguientes:  


-   [Unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Administración de riesgos con control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configure directivas de autenticación mediante el\-de AD FS de administración en  
El requisito mínimo para realizar estos procedimientos es la pertenencia al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   

En AD FS, en Windows Server 2012 R2, puede especificar una directiva de autenticación en un ámbito global que se aplique a todas las aplicaciones y servicios que están protegidos por AD FS. También puede establecer directivas de autenticación para aplicaciones y servicios específicos que se basan en las relaciones de confianza de entidades y están protegidos por AD FS. La especificación de una directiva de autenticación para una aplicación determinada por relación de confianza para usuario autenticado no invalida la Directiva de autenticación global. Si una directiva de autenticación global o por relación de confianza para usuario autenticado requiere MFA, se desencadena MFA cuando el usuario intenta autenticarse en esta relación de confianza para usuario autenticado. La Directiva de autenticación global es una reserva para las relaciones de confianza para usuario autenticado para aplicaciones y servicios que no tienen una directiva de autenticación configurada concreta. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Para configurar la autenticación principal globalmente en Windows Server 2012 R2 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En AD FS\-de complemento en, haga clic en **directivas de autenticación**.  

3.  En la sección **autenticación principal** , haga clic en **Editar** junto a **configuración global**. También puede hacer clic con el botón derecho en\-**directivas de autenticación**y seleccionar **Editar autenticación principal global**, o bien, en el panel **acciones** , seleccione **Editar autenticación principal global**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy1.png)

4.  En la ventana **Editar Directiva de autenticación global** , en la pestaña **principal** , puede configurar las siguientes opciones como parte de la Directiva de autenticación global:  

    -   Métodos de autenticación que se usarán para la autenticación principal. Puede seleccionar métodos de autenticación disponibles en la **extranet** y la **intranet**.  

    -   Autenticación de dispositivos mediante la casilla **Habilitar autenticación de dispositivo** . Para obtener más información, consulte [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Para configurar la autenticación principal por relación de confianza para usuario autenticado  

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En AD FS\-de complemento en, haga clic en **directivas de autenticación**\\por relación de confianza para usuario **autenticado**y, a continuación, haga clic en la relación de confianza para la que desea configurar las directivas de autenticación.  

3.  \-haga clic con el botón derecho en la relación de usuario de confianza para la que desea configurar las directivas de autenticación y, después, seleccione **Editar autenticación principal personalizada**, o bien, en el panel **acciones** , seleccione **Editar autenticación principal personalizada**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  En la ventana **Editar Directiva de autenticación para < que confía\_entidad\_confianza\_nombre >** , en la pestaña **principal** , puede configurar el siguiente valor como parte de la Directiva de autenticación **por relación de confianza para usuario autenticado** :  

    -   Si los usuarios deben proporcionar sus credenciales cada vez que se\-el inicio de sesión a través de, los **usuarios deben proporcionar sus credenciales cada vez en la casilla firmar\-de** .  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Para configurar la autenticación multifactor globalmente  

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En AD FS\-de complemento en, haga clic en **directivas de autenticación**.  

3.  En la sección **autenticación de Multi\-factor Authentication** , haga clic en **Editar** junto a **configuración global**. También puede hacer clic con el botón derecho en\-**directivas de autenticación**y seleccionar **Editar autenticación global multi\-factor**, o bien, en el panel **acciones** , seleccione **Editar autenticación global multi\-factor Authentication**.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  En la ventana **Editar Directiva de autenticación global** , en la pestaña **factor de\-múltiples** , puede configurar las siguientes opciones como parte de la Directiva de autenticación global multi\-factor:  

    -   Configuración o condiciones para MFA a través de las opciones disponibles en las secciones **usuarios\/grupos**, **dispositivos**y **ubicaciones** .  

    -   Para habilitar MFA para cualquiera de estas opciones, debe seleccionar al menos un método de autenticación adicional. La **autenticación de certificado** es la opción disponible de forma predeterminada. También puede configurar otros métodos de autenticación adicionales personalizados, por ejemplo, la autenticación activa de Windows Azure. Para obtener más información, consulte [Guía de tutorial: administración de riesgos con multi-factor Authentication adicionales para aplicaciones confidenciales](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  

> [!WARNING]  
> Solo puede configurar métodos de autenticación adicionales de forma global.  
![directivas de autenticación](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Para configurar multi\-factor Authentication por relación de confianza para usuario autenticado  

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  

2.  En AD FS\-de ajuste en, haga clic en **directivas de autenticación**\\por relación de confianza para usuario **autenticado**y, a continuación, haga clic en la relación de confianza para la que desea configurar MFA.  

3.  \-haga clic con el botón derecho en la relación de usuario de confianza para la que desea configurar MFA y, a continuación, seleccione **Editar autenticación personalizada multi\-factor Authentication**, o bien, en el panel **acciones** , seleccione **Editar autenticación personalizada multi\-factor Authentication**.  

4.  En la ventana **Editar Directiva de autenticación para < que confía\_entidad\_confianza\_nombre >** , en la pestaña **factor de\-múltiple** , puede configurar las siguientes opciones como parte de la Directiva de autenticación de relación de confianza para usuario autenticado por\-:  

    -   Configuración o condiciones para MFA a través de las opciones disponibles en las secciones **usuarios\/grupos**, **dispositivos**y **ubicaciones** .  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurar directivas de autenticación mediante Windows PowerShell  
Windows PowerShell ofrece una mayor flexibilidad a la hora de usar varios factores de control de acceso y el mecanismo de autenticación que están disponibles en AD FS en Windows Server 2012 R2 para configurar directivas de autenticación y reglas de autorización que son necesarias para implementar el acceso condicional real para el AD FS \-recursos protegidos.  

El requisito mínimo para realizar estos procedimientos es la pertenencia al grupo Administradores o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   

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
> Asegúrese de reemplazar *< confianza de\_de usuario\_>* por el nombre de la relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  


~~~
$MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'”, Value =~ ‘“^(?i) <group_SID>$'”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn'”);” 

Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
~~~


> [!NOTE]  
> Asegúrese de reemplazar el > de SID\_de grupo de < por el valor del identificador de seguridad \(SID\) del grupo Active Directory \(AD\).  

### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Para configurar MFA globalmente en función de los datos de pertenencia a grupos de los usuarios  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value == ‘"group_SID'"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< grupo\_sid >* por el valor del SID del grupo de ad.  

### <a name="to-configure-mfa-globally-based-on-users-location"></a>Para configurar MFA globalmente en función de la ubicación del usuario  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == ‘"true_or_false'"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~



> [!NOTE]  
> Asegúrese de reemplazar *< true\_o\_falso >* por `true` o `false`. El valor depende de la condición de regla específica que se basa en si la solicitud de acceso procede de la extranet o de la intranet.  

### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Para configurar MFA globalmente según los datos del dispositivo del usuario  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
$MfaClaimRule = "c:[Type == ‘" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == ‘"true_or_false"']  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< true\_o\_falso >* por `true` o `false`. El valor depende de la condición de regla específica que se basa en si el dispositivo es Workplace\-o no Unido.  

### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Para configurar MFA globalmente si la solicitud de acceso procede de la extranet y de un dispositivo unido a un área de trabajo que no es\-\-  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
`Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
~~~


> [!NOTE]  
> Asegúrese de reemplazar ambas instancias de *< true\_o\_falso >* con `true` o `false`, que depende de las condiciones de regla específicas. Las condiciones de la regla se basan en si el dispositivo es el área de trabajo\-unida o no, y si la solicitud de acceso procede de la extranet o la intranet.  

### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Para configurar MFA globalmente si el acceso procede de un usuario de extranet que pertenece a un grupo determinado  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
~~~

> [!NOTE]  
> Asegúrese de reemplazar *< > del grupo\_SID* por el valor del SID del grupo y *< true\_o\_false >* con `true` o `false`, que depende de la condición de regla específica que se basa en si la solicitud de acceso procede de la extranet o la intranet.  

### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Para conceder acceso a una aplicación basada en datos de usuario a través de Windows PowerShell  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Asegúrese de reemplazar *< confianza de\_de usuario\_>* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  

   ```  

     $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
   Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
   ```  

> [!NOTE]  
> > Asegúrese de reemplazar *< grupo\_sid >* por el valor del SID del grupo de ad.  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Para conceder acceso a una aplicación protegida por AD FS solo si la identidad de este usuario se validó con MFA  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< confianza de\_de usuario\_>* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  

   ```  
   $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
   @RuleName = `"PermitAccessWithMFA`"  
   c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim'");"  

   ```  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Para conceder acceso a una aplicación protegida por AD FS solo si la solicitud de acceso procede de un área de trabajo\-dispositivo Unido que está registrado para el usuario  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Asegúrese de reemplazar *< confianza de\_de usuario\_>* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
~~~



### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acceso a una aplicación protegida por AD FS solo si la solicitud de acceso procede de un área de trabajo\-dispositivo Unido registrado a un usuario cuya identidad se ha validado con MFA  

1.  En el servidor de Federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Asegúrese de reemplazar *< confianza de\_de usuario\_>* por el valor de su relación de confianza para usuario autenticado.  

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
> Asegúrese de reemplazar *< confianza de\_de usuario\_>* por el valor de su relación de confianza para usuario autenticado.  

2. En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"RequireMFAForExtranetAccess`"  
c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
~~~

## <a name="additional-references"></a>Referencias adicionales  

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
