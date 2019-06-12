---
title: Compuesta de servicios de dominio de Active Directory y autenticación de notificaciones en servicios de federación de Active Directory
description: El siguiente documento describe la autenticación compuesta y notificaciones de AD DS en AD FS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2380060894ff2f365451bbabfd41b8aa7e6792a0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445296"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Autenticación compuesta y notificaciones de AD DS en AD FS
Windows Server 2012 mejora la autenticación Kerberos mediante la introducción de autenticación compuesta.  Autenticación compuesta permite realizar una solicitud de servicio de concesión de vales de Kerberos (TGS) incluir dos identidades: 

- la identidad del usuario
- la identidad del dispositivo del usuario.  

Windows lleva a cabo la autenticación compuesta mediante la extensión [Kerberos Authentication túnel seguro Flexible (FAST), o la protección de Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 y versiones posteriores permite el consumo de AD DS emite notificaciones de usuario o dispositivo que residan en un vale de autenticación de Kerberos. En versiones anteriores de AD FS, las notificaciones motor leyera solo usuario y grupo identificadores de seguridad (SID) de Kerberos, pero no pudo leer cualquier información contenida en un vale de Kerberos de notificaciones.

Puede permitir un mayor control de acceso para las aplicaciones federadas mediante el uso de servicios de dominio de Active Directory (AD DS)-emite notificaciones de usuario y dispositivo juntos, con los servicios de federación de Active Directory (AD FS).

## <a name="requirements"></a>Requisitos
1.  Los equipos que acceden a aplicaciones federadas, se debe autenticar en AD FS usando **autenticación integrada de Windows**. 
    - La autenticación integrada de Windows solo está disponible cuando se conecta a los servidores back-end de AD FS.
    - Los equipos deben ser capaces de conectarse a los servidores back-end de AD FS para el nombre del servicio de federación
    - Servidores de AD FS debe ofrecer autenticación integrada de Windows como un método de autenticación principal en su configuración de la Intranet.

2.  La directiva **compatibilidad con clientes de Kerberos para autenticación de notificaciones compuesta y protección de Kerberos** debe aplicarse a todos los equipos que tienen acceso a aplicaciones federadas que están protegidas por autenticación compuesta. Esto es aplicable en el caso de bosque único o escenarios de bosque entre.

3.  El dominio que aloja los servidores de AD FS debe tener la **KDC de compatibilidad con notificaciones autenticación compuesta y protección de Kerberos** configuración de directiva que se aplica a los controladores de dominio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Pasos para configurar AD FS en Windows Server 2012 R2
Siga estos pasos para configurar notificaciones y autenticación compuesta 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Paso 1:  Habilitar la compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos en la directiva predeterminada de controladores de dominio
1.  En el administrador del servidor, seleccione Herramientas, **Group Policy Management**.
2.  Desplácese hacia abajo hasta la **directiva predeterminada de controladores de dominio**, haga clic en y seleccione **editar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  En el **Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas** , expanda **sistema**y seleccione **KDC**.
4.  En el panel derecho, haga doble clic en **compatibilidad del KDC con notificaciones, autenticación compuesta y protección de Kerberos**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  En la nueva ventana del cuadro de diálogo, establezca compatibilidad de KDC para notificaciones **habilitado**.
6.  En Opciones, seleccione **admitidos** desde el menú desplegable y, a continuación, haga clic en **aplicar** y **Aceptar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Paso 2: Habilitar la compatibilidad de cliente de Kerberos para notificaciones, autenticación compuesta y protección de Kerberos en equipos que acceden a aplicaciones federadas

1.  En una directiva de grupo que se aplican a los equipos acceso a aplicaciones federadas, en el **Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **sistema**y seleccione **Kerberos**.
2.  En el panel derecho de la ventana del Editor de administración de directivas de grupo, haga doble clic en **compatibilidad con clientes de Kerberos para notificaciones, autenticación compuesta y protección de Kerberos.**
3.  En la nueva ventana del cuadro de diálogo, establezca la compatibilidad con cliente Kerberos en **habilitado** y haga clic en **aplicar** y **Aceptar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Cierra el Editor de administración de directivas de grupo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Paso 3: Asegúrese de que se han actualizado los servidores de AD FS.
Deberá asegurarse de que las actualizaciones siguientes están instaladas en los servidores de AD FS.

|Actualización|Descripción|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Actualización de seguridad acumulativa (incluye KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Actualización de Server 2012 R2|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Esta actualización agrega compatibilidad con notificaciones de identificador compuestas en servicios de federación de Active Directory.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Paso 4: Configurar el proveedor de autenticación principal

1. Establece el proveedor de autenticación principal en **Windows autenticación** para la configuración de la Intranet de AD FS.
2. Administración de AD FS, en **las directivas de autenticación**, seleccione **autenticación principal** y en **configuración Global** haga clic en **editar**.
3. En **Editar directiva de autenticación Global** en **Intranet** seleccione **Windows autenticación**.
4. Haga clic en **aplicar** y **Aceptar**.

![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Uso de PowerShell que puede usar el **Set-AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>En WID basada en conjunto, el comando debe ejecutarse en el servidor de AD FS principal de PowerShell.
>En una granja de servidores basado en SQL, se puede ejecutar el comando de PowerShell en cualquier servidor de AD FS es miembro de la granja de servidores.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Paso 5:  Agregue la descripción de notificación a AD FS
1. Agregue la siguiente descripción de notificación a la granja. Esta descripción de notificación no está presente de forma predeterminada en AD FS 2012 R2 y debe agregarse manualmente.
2. Administración de AD FS, en **servicio**, haga clic en **descripción de notificación** y seleccione **Agregar descripción de notificación**
3. Escriba la siguiente información en la descripción de notificación
   - Nombre para mostrar: 'Grupo de dispositivos de Windows' 
   - Descripción de notificación: '<https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup>' '
4. Coloque una marca de verificación en ambos cuadros.
5. Haga clic en **Aceptar**.

![Descripción de notificación.](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Uso de PowerShell que puede usar el **agregar AdfsClaimDescription** cmdlet.
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
   ```


>[!NOTE]
>En WID basada en conjunto, el comando debe ejecutarse en el servidor de AD FS principal de PowerShell.
>En una granja de servidores basado en SQL, se puede ejecutar el comando de PowerShell en cualquier servidor de AD FS es miembro de la granja de servidores.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Paso 6:  Habilitar el bit de autenticación compuesta en el atributo msDS-SupportedEncryptionTypes

1.  Habilitar la autenticación compuesta de bits en el atributo msDS-SupportedEncryptionTypes de la cuenta designada para ejecutar el servicio de AD FS mediante el **Set-ADServiceAccount** cmdlet de PowerShell.  

>[!NOTE]
>Si cambia la cuenta de servicio, debe habilitar manualmente la autenticación compuesta mediante la ejecución de la **compoundIdentitySupported - Set-ADUser: $true** cmdlets de Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Reinicie el servicio ADFS.

>[!NOTE]
>Una vez 'CompoundIdentitySupported' está establecida en true, instalación de la misma gMSA en nuevos servidores (2012 R2/2016) produce un error con el siguiente error: **Install-ADServiceAccount: No se puede instalar la cuenta de servicio. Mensaje de error: "El contexto proporcionado no coincidió con el destino."** .
>
>**Solución**: Establezca temporalmente CompoundIdentitySupported en $false. Este paso hace que ADFS detener la emisión de notificaciones WindowsDeviceGroup. Set-ADServiceAccount-Identity 'Cuenta de servicio de ADFS' - CompoundIdentitySupported: $false instalar la gMSA en el nuevo servidor y, a continuación, habilitar CompoundIdentitySupported a $True.
Deshabilitar CompoundIdentitySupported y, a continuación, volver a habilitar no es necesario reiniciar servicio de ADFS.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Paso 7: Confianza de proveedor de notificaciones de actualización de la instancia de AD FS para Active Directory

1. Actualizar el AD FS notificaciones una confianza del proveedor para que Active Directory incluir la siguiente regla de notificación de 'Paso a través' para 'WindowsDeviceGroup' notificación.
2.  En **administración de AD FS**, haga clic en **confianzas de proveedor de notificaciones** y en el panel derecho, con el botón derecho y haga clic en **Active Directory** y seleccione **editar reglas de notificación**.
3.  En el **editar reglas de notificación para Active Directory** haga clic en **Agregar regla**.
4.  En el **transformar notificaciones Asistente para agregar regla** seleccione **pasar o filtrar una notificación entrante** y haga clic en **siguiente**.
5.  Agregar un nombre para mostrar y seleccionar **grupo de dispositivos de Windows** desde el **tipo de notificación entrante** lista desplegable.
6.  Haga clic en **Finalizar**.  Haga clic en **aplicar** y **Aceptar**. 
![Descripción de notificación.](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Paso 8: En la parte que confía donde se esperan que las notificaciones de 'WindowsDeviceGroup', agregue una regla de notificación de 'Paso a través' o 'Transformar' similar.
2. En **administración de AD FS**, haga clic en **autenticado** y en el panel derecho, con el botón derecho y haga clic en el RP y selecciona **editar reglas de notificación**.
3. En el **reglas de transformación de emisión** haga clic en **Agregar regla**.
4. En el **transformar notificaciones Asistente para agregar regla** seleccione **pasar o filtrar una notificación entrante** y haga clic en **siguiente**.
5. Agregar un nombre para mostrar y seleccionar **grupo de dispositivos de Windows** desde el **tipo de notificación entrante** lista desplegable.
6. Haga clic en **Finalizar**.  Haga clic en **aplicar** y **Aceptar**.
   ![Descripción de notificación.](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Pasos para configurar AD FS en Windows Server 2016
Lo siguiente detalla los pasos para configurar la autenticación compuesta en AD FS para Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Paso 1:  Habilitar la compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos en la directiva predeterminada de controladores de dominio
1.  En el administrador del servidor, seleccione Herramientas, **Group Policy Management**.
2.  Desplácese hacia abajo hasta la **directiva predeterminada de controladores de dominio**, haga clic en y seleccione **editar**.
3.  En el **Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas** , expanda **sistema**y seleccione **KDC**.
4.  En el panel derecho, haga doble clic en **compatibilidad del KDC con notificaciones, autenticación compuesta y protección de Kerberos**.
5.  En la nueva ventana del cuadro de diálogo, establezca compatibilidad de KDC para notificaciones **habilitado**.
6.  En Opciones, seleccione **admitidos** desde el menú desplegable y, a continuación, haga clic en **aplicar** y **Aceptar**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Paso 2: Habilitar la compatibilidad de cliente de Kerberos para notificaciones, autenticación compuesta y protección de Kerberos en equipos que acceden a aplicaciones federadas

1.  En una directiva de grupo que se aplican a los equipos acceso a aplicaciones federadas, en el **Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **sistema**y seleccione **Kerberos**.
2.  En el panel derecho de la ventana del Editor de administración de directivas de grupo, haga doble clic en **compatibilidad con clientes de Kerberos para notificaciones, autenticación compuesta y protección de Kerberos.**
3.  En la nueva ventana del cuadro de diálogo, establezca la compatibilidad con cliente Kerberos en **habilitado** y haga clic en **aplicar** y **Aceptar**.
4.  Cierra el Editor de administración de directivas de grupo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Paso 3: Configurar el proveedor de autenticación principal

1. Establece el proveedor de autenticación principal en **Windows autenticación** para la configuración de la Intranet de AD FS.
2. Administración de AD FS, en **las directivas de autenticación**, seleccione **autenticación principal** y en **configuración Global** haga clic en **editar**.
3. En **Editar directiva de autenticación Global** en **Intranet** seleccione **Windows autenticación**.
4. Haga clic en **aplicar** y **Aceptar**.
5. Uso de PowerShell que puede usar el **Set-AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>En WID basada en conjunto, el comando debe ejecutarse en el servidor de AD FS principal de PowerShell.
>En una granja de servidores basado en SQL, se puede ejecutar el comando de PowerShell en cualquier servidor de AD FS es miembro de la granja de servidores.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Paso 4:  Habilitar el bit de autenticación compuesta en el atributo msDS-SupportedEncryptionTypes

1.  Habilitar la autenticación compuesta de bits en el atributo msDS-SupportedEncryptionTypes de la cuenta designada para ejecutar el servicio de AD FS mediante el **Set-ADServiceAccount** cmdlet de PowerShell.  

>[!NOTE]
>Si cambia la cuenta de servicio, debe habilitar manualmente la autenticación compuesta mediante la ejecución de la **compoundIdentitySupported - Set-ADUser: $true** cmdlets de Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Reinicie el servicio ADFS.

>[!NOTE]
>Una vez 'CompoundIdentitySupported' está establecida en true, instalación de la misma gMSA en nuevos servidores (2012 R2/2016) produce un error con el siguiente error: **Install-ADServiceAccount: No se puede instalar la cuenta de servicio. Mensaje de error: "El contexto proporcionado no coincidió con el destino."** .
>
>**Solución**: Establezca temporalmente CompoundIdentitySupported en $false. Este paso hace que ADFS detener la emisión de notificaciones WindowsDeviceGroup. Set-ADServiceAccount-Identity 'Cuenta de servicio de ADFS' - CompoundIdentitySupported: $false instalar la gMSA en el nuevo servidor y, a continuación, habilitar CompoundIdentitySupported a $True.
Deshabilitar CompoundIdentitySupported y, a continuación, volver a habilitar no es necesario reiniciar servicio de ADFS.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Paso 5: Confianza de proveedor de notificaciones de actualización de la instancia de AD FS para Active Directory

1. Actualizar el AD FS notificaciones una confianza del proveedor para que Active Directory incluir la siguiente regla de notificación de 'Paso a través' para 'WindowsDeviceGroup' notificación.
2.  En **administración de AD FS**, haga clic en **confianzas de proveedor de notificaciones** y en el panel derecho, con el botón derecho y haga clic en **Active Directory** y seleccione **editar reglas de notificación**.
3.  En el **editar reglas de notificación para Active Directory** haga clic en **Agregar regla**.
4.  En el **transformar notificaciones Asistente para agregar regla** seleccione **pasar o filtrar una notificación entrante** y haga clic en **siguiente**.
5.  Agregar un nombre para mostrar y seleccionar **grupo de dispositivos de Windows** desde el **tipo de notificación entrante** lista desplegable.
6.  Haga clic en **Finalizar**.  Haga clic en **aplicar** y **Aceptar**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Paso 6: En la parte que confía donde se esperan que las notificaciones de 'WindowsDeviceGroup', agregue una regla de notificación de 'Paso a través' o 'Transformar' similar.
2. En **administración de AD FS**, haga clic en **autenticado** y en el panel derecho, con el botón derecho y haga clic en el RP y selecciona **editar reglas de notificación**.
3. En el **reglas de transformación de emisión** haga clic en **Agregar regla**.
4. En el **transformar notificaciones Asistente para agregar regla** seleccione **pasar o filtrar una notificación entrante** y haga clic en **siguiente**.
5. Agregar un nombre para mostrar y seleccionar **grupo de dispositivos de Windows** desde el **tipo de notificación entrante** lista desplegable.
6. Haga clic en **Finalizar**.  Haga clic en **aplicar** y **Aceptar**.

## <a name="validation"></a>Resultados
Para validar la versión de 'WindowsDeviceGroup' reclamaciones, crear una prueba de notificaciones de aplicación compatible con .net 4.6. Con el SDK de WIF 4.0.
Configurar la aplicación como un usuario de confianza de ADFS y actualícelo con la regla de notificación como se especifica en los pasos anteriores.
Al autenticarse en la aplicación con el proveedor de autenticación integrada de Windows de AD FS, las siguientes notificaciones son convertidas.
![Validación](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Ahora se pueden utilizar las notificaciones para el equipo o dispositivo para los controles de acceso más completos.

Por ejemplo, la siguiente **AdditionalAuthenticationRules** indica a AD FS para invocar MFA si: el usuario de autenticación no es miembro del grupo de seguridad "-1-5-21-2134745077-1211275016-3050530490-1117" y el equipo (donde es el usuario es la autenticación de) no es miembro del grupo de seguridad "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

Sin embargo, si se cumple alguna de las condiciones anteriores, invocar MFA.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
