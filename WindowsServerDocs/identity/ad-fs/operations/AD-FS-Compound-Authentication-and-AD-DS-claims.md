---
title: "Compuesta solicitudes de autenticación y los servicios de dominio de Active Directory en los servicios de federación de Active Directory"
description: "El siguiente documento describe la autenticación compuesta y reclamaciones de AD DS en AD FS."
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5f80cbcbdb2deca9b098014627365b8ea819b5f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Compuesta de autenticación y reclamaciones de AD DS en AD FS
Windows Server 2012 mejora la autenticación Kerberos al introducir la autenticación compuesta.  La autenticación compuesta permite una solicitud de servicio de concesión de vales Kerberos (TGS) para incluir dos identidades: 

- La identidad del usuario
- La identidad del dispositivo del usuario.  

Windows lleva a cabo la autenticación compuesta extendiendo [Kerberos Flexible autenticación segura de túnel (rápido) o la protección de Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 y versiones posteriores permite consumo de AD DS emitido reclamaciones de usuario o dispositivo que residen en un vale de autenticación de Kerberos. En versiones anteriores de AD FS, las notificaciones de las notificaciones motor pudo leer solo usuario y grupo identificadores de seguridad (SID) de Kerberos, pero no pudo leer cualquier información contenida en un vale Kerberos.

Puedes habilitar el control de acceso más enriquecido para aplicaciones federados mediante el uso de los servicios de dominio de Active Directory (AD DS)-emitido reclamaciones de usuarios y dispositivos entre sí, con los servicios de federación de Active Directory (AD FS).

## <a name="requirements"></a>Requisitos
1.  Los equipos que tienen acceso a aplicaciones federadas, debe autenticarse en el uso de AD FS **autenticación integrada en Windows**. 
    - Autenticación integrada de Windows solo está disponible cuando se conecta a los servidores de back-end AD FS.
    - Equipos deben ser capaz de alcanzar los servidores de back-end AD FS federación nombre del servicio
    - Los servidores de AD FS debe ofrecer la autenticación integrada de Windows como método de autenticación principal en su configuración de la Intranet.

2.  La directiva **compatibilidad de cliente de Kerberos para autenticación compuesta de notificaciones y protección de Kerberos** debe aplicarse a todos los equipos que tienen acceso a aplicaciones federadas que están protegidas por la autenticación compuesta. Esto es aplicable en el caso de bosque único o entre los escenarios de bosque.

3.  El dominio de la caja los servidores de AD FS debe tener la **KDC soporte técnico para reclamaciones compuestos autenticación y protección de Kerberos** configuración de directiva que se aplica a los controladores de dominio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Pasos para configurar AD FS en Windows Server 2012 R2
Usa los siguientes pasos para configurar la autenticación compuesta y notificaciones 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Paso 1: Habilitar la compatibilidad KDC para notificaciones, autenticación compuesta y protección de Kerberos en la directiva de controlador de dominio predeterminada
1.  En el administrador del servidor, selecciona Herramientas, **Group Policy Management**.
2.  Desplazarse hacia abajo hasta la **directiva predeterminada de controladores de dominio**, haz clic derecho y selecciona **editar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  En la **el Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expande **plantillas administrativas**, expande **sistema**y selecciona **KDC**.
4.  En el panel derecho, haz doble clic en **compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  En la nueva ventana del cuadro de diálogo, establece compatibilidad KDC para notificaciones para **habilitado**.
6.  En Opciones, selecciona **Supported** desde el menú desplegable y, a continuación, haz clic en **aplicar** y **Aceptar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Paso 2: Habilitar la compatibilidad de cliente de Kerberos para notificaciones, autenticación compuesta y protección de Kerberos en equipos que tienen acceso a aplicaciones federadas

1.  En una directiva de grupo que se aplican a los equipos acceso a aplicaciones federadas, en la **el Editor de administración de directivas de grupo**, en **configuración del equipo**, expande **directivas**, expande **administrativo Plantillas**, expanda **sistema**y selecciona **Kerberos**.
2.  En el panel derecho de la ventana del Editor de administración de directivas de grupo, haz doble clic en **compatibilidad del cliente Kerberos con reclamaciones, autenticación compuesta y protección de Kerberos.**
3.  En la nueva ventana del cuadro de diálogo, Establece la compatibilidad con el cliente Kerberos en **habilitado** y haz clic en **aplicar** y **Aceptar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Cierra el Editor de administración de directivas de grupo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Paso 3: Garantizar que se han actualizado los servidores de AD FS.
Debes asegurarte de que se instalan las actualizaciones siguientes en los servidores de AD FS.

|Actualización|Descripción|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Actualización de seguridad acumulativa (incluye KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Actualización para Server 2012 R2|
|[Revisión 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Esta actualización agrega compatibilidad para compuestos reclamaciones de identificador en los servicios de federación de Active Directory.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Paso 4: Configurar el proveedor de autenticación principal

1. Establece el proveedor de autenticación principal en **autenticación de Windows** para la configuración de la Intranet de AD FS.
2. En la administración de AD FS, bajo **directivas de autenticación**, selecciona **autenticación principal** y, en **configuración Global** haga clic en **editar**.
3. En **Editar directiva de autenticación Global** en **Intranet** selecciona **autenticación de Windows**.
4. Haz clic en **aplicar** y **Aceptar**.

![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Uso de PowerShell, puedes usar la **conjunto AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>En un WID basa granja de servidores, el comando debe ejecutarse en el servidor AD FS principal de PowerShell.
>En una granja de servidores en función de SQL, el comando de PowerShell puede ejecutarse en cualquier servidor de AD FS que es un miembro de la granja de servidores.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Paso 5: Agregar la descripción de la notificación a AD FS
1.  Agrega la siguiente descripción de la notificación a la granja de servidores. Esta descripción de la notificación no está presente de manera predeterminada en ADFS 2012 R2 y se debe agregar manualmente.
2.  En la administración de AD FS, bajo **servicio**, haz clic en **reclamar descripción** y selecciona **agregar reclamar descripción**
3.  Escribe la siguiente información en la descripción de la notificación
    - Nombre para mostrar: 'Windows dispositivo grupo' 
    - Descripción de la reclamación: 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' '
4. Coloca una comprobación en ambos.
5. Haz clic en **Aceptar**.

![Descripción de la notificación](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Uso de PowerShell, puedes usar la **agregar AdfsClaimDescription** cmdlet.
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>En un WID basa granja de servidores, el comando debe ejecutarse en el servidor AD FS principal de PowerShell.
>En una granja de servidores en función de SQL, el comando de PowerShell puede ejecutarse en cualquier servidor de AD FS que es un miembro de la granja de servidores.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Paso 6: Habilitar el bit de autenticación compuesta en el atributo msDS SupportedEncryptionTypes

1.  Habilitar la autenticación compuesta bit en el atributo msDS SupportedEncryptionTypes la cuenta designados para ejecutar el servicio de AD FS usando el **conjunto ADServiceAccount** cmdlet de PowerShell.  

>[!NOTE]
>Si cambias la cuenta de servicio, debe habilitar la autenticación compuesta manualmente mediante la ejecución de la **Set-ADUser - compoundIdentitySupported: $true** cmdlets de Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Reiniciar el servicio ADFS.

>[!NOTE]
>Una vez 'CompoundIdentitySupported' se establece en true, instalación de la misma gMSA en nueva servidores (2012R2/2016) se produce un error con el siguiente error: **instalación ADServiceAccount: no puedes instalar la cuenta de servicio. Mensaje de error: "el contexto proporcionado no coincidió con el destino."**.
>
>**Solución**: establecer temporalmente CompoundIdentitySupported $false. Este paso hace ADFS dejar de emitir WindowsDeviceGroup reclamaciones. Set-ADServiceAccount-identidad 'Cuenta de servicio ADFS' - CompoundIdentitySupported: $false instalar el gMSA en el servidor nuevo y, a continuación, habilitar CompoundIdentitySupported volver a $True.
Deshabilitar CompoundIdentitySupported y, a continuación, volver a habilitar no es necesario reiniciar servicio ADFS.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Paso 7: Actualizar el proveedor de notificaciones de AD FS de confianza para Active Directory

1. Actualización de AD FS reclamaciones de proveedor de confianza para que Active Directory para incluir la siguiente regla de Reclamación 'Paso', 'WindowsDeviceGroup' reclamación.
2.  En **AD FS administración**, haz clic en **reclamaciones proveedor confía** y en el panel derecho, haga clic **Active Directory** y selecciona **editar reglas de notificación**.
3.  En la **editar reglas de notificación para el Director de activo** haga clic en **Agregar regla**.
4.  En la **transformar reclamación regla Asistente para agregar** selecciona **paso a través o una notificación entrante de filtro** y haz clic en **siguiente**.
5.  Agregar un nombre para mostrar y selecciona **grupo de dispositivos de Windows** desde el **tipo de notificación entrante** lista desplegable.
6.  Haz clic en **finalizar**.  Haz clic en **aplicar** y **Aceptar**. 
![Descripción de la notificación](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Paso 8: En la parte confiar donde se esperan que las notificaciones 'WindowsDeviceGroup', agrega una regla similar de Reclamación de 'Paso a través' o 'Transformar'.
2.  En **AD FS administración**, haz clic en **confiar confía en las partes** y en el panel derecho, haga clic en el punto de reunión y selecciona **editar reglas de notificación**.
3.  En la **reglas de transformación de emisión** haga clic en **Agregar regla**.
4.  En la **transformar reclamación regla Asistente para agregar** selecciona **paso a través o una notificación entrante de filtro** y haz clic en **siguiente**.
5.  Agregar un nombre para mostrar y selecciona **grupo de dispositivos de Windows** desde el **tipo de notificación entrante** lista desplegable.
6.  Haz clic en **finalizar**.  Haz clic en **aplicar** y **Aceptar**.
![Descripción de la notificación](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Pasos para configurar AD FS en Windows Server 2016
El siguiente detallan los pasos para configurar la autenticación compuesta en AD FS de Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Paso 1: Habilitar la compatibilidad KDC para notificaciones, autenticación compuesta y protección de Kerberos en la directiva de controlador de dominio predeterminada
1.  En el administrador del servidor, selecciona Herramientas, **Group Policy Management**.
2.  Desplazarse hacia abajo hasta la **directiva predeterminada de controladores de dominio**, haz clic derecho y selecciona **editar**.
3.  En la **el Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expande **plantillas administrativas**, expande **sistema**y selecciona **KDC**.
4.  En el panel derecho, haz doble clic en **compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos**.
5.  En la nueva ventana del cuadro de diálogo, establece compatibilidad KDC para notificaciones para **habilitado**.
6.  En Opciones, selecciona **Supported** desde el menú desplegable y, a continuación, haz clic en **aplicar** y **Aceptar**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Paso 2: Habilitar la compatibilidad de cliente de Kerberos para notificaciones, autenticación compuesta y protección de Kerberos en equipos que tienen acceso a aplicaciones federadas

1.  En una directiva de grupo que se aplican a los equipos acceso a aplicaciones federadas, en la **el Editor de administración de directivas de grupo**, en **configuración del equipo**, expande **directivas**, expande **administrativo Plantillas**, expanda **sistema**y selecciona **Kerberos**.
2.  En el panel derecho de la ventana del Editor de administración de directivas de grupo, haz doble clic en **compatibilidad del cliente Kerberos con reclamaciones, autenticación compuesta y protección de Kerberos.**
3.  En la nueva ventana del cuadro de diálogo, Establece la compatibilidad con el cliente Kerberos en **habilitado** y haz clic en **aplicar** y **Aceptar**.
4.  Cierra el Editor de administración de directivas de grupo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Paso 3: Configurar el proveedor de autenticación principal

1. Establece el proveedor de autenticación principal en **autenticación de Windows** para la configuración de la Intranet de AD FS.
2. En la administración de AD FS, bajo **directivas de autenticación**, selecciona **autenticación principal** y, en **configuración Global** haga clic en **editar**.
3. En **Editar directiva de autenticación Global** en **Intranet** selecciona **autenticación de Windows**.
4. Haz clic en **aplicar** y **Aceptar**.
5. Uso de PowerShell, puedes usar la **conjunto AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>En un WID basa granja de servidores, el comando debe ejecutarse en el servidor AD FS principal de PowerShell.
>En una granja de servidores en función de SQL, el comando de PowerShell puede ejecutarse en cualquier servidor de AD FS que es un miembro de la granja de servidores.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Paso 4: Habilitar el bit de autenticación compuesta en el atributo msDS SupportedEncryptionTypes

1.  Habilitar la autenticación compuesta bit en el atributo msDS SupportedEncryptionTypes la cuenta designados para ejecutar el servicio de AD FS usando el **conjunto ADServiceAccount** cmdlet de PowerShell.  

>[!NOTE]
>Si cambias la cuenta de servicio, debe habilitar la autenticación compuesta manualmente mediante la ejecución de la **Set-ADUser - compoundIdentitySupported: $true** cmdlets de Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Reiniciar el servicio ADFS.

>[!NOTE]
>Una vez 'CompoundIdentitySupported' se establece en true, instalación de la misma gMSA en nueva servidores (2012R2/2016) se produce un error con el siguiente error: **instalación ADServiceAccount: no puedes instalar la cuenta de servicio. Mensaje de error: "el contexto proporcionado no coincidió con el destino."**.
>
>**Solución**: establecer temporalmente CompoundIdentitySupported $false. Este paso hace ADFS dejar de emitir WindowsDeviceGroup reclamaciones. Set-ADServiceAccount-identidad 'Cuenta de servicio ADFS' - CompoundIdentitySupported: $false instalar el gMSA en el servidor nuevo y, a continuación, habilitar CompoundIdentitySupported volver a $True.
Deshabilitar CompoundIdentitySupported y, a continuación, volver a habilitar no es necesario reiniciar servicio ADFS.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Paso 5: Actualizar el proveedor de notificaciones de AD FS de confianza para Active Directory

1. Actualización de AD FS reclamaciones de proveedor de confianza para que Active Directory para incluir la siguiente regla de Reclamación 'Paso', 'WindowsDeviceGroup' reclamación.
2.  En **AD FS administración**, haz clic en **reclamaciones proveedor confía** y en el panel derecho, haga clic **Active Directory** y selecciona **editar reglas de notificación**.
3.  En la **editar reglas de notificación para el Director de activo** haga clic en **Agregar regla**.
4.  En la **transformar reclamación regla Asistente para agregar** selecciona **paso a través o una notificación entrante de filtro** y haz clic en **siguiente**.
5.  Agregar un nombre para mostrar y selecciona **grupo de dispositivos de Windows** desde el **tipo de notificación entrante** lista desplegable.
6.  Haz clic en **finalizar**.  Haz clic en **aplicar** y **Aceptar**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Paso 6: En la parte confiar donde se esperan que las notificaciones 'WindowsDeviceGroup', agrega una regla similar de Reclamación de 'Paso a través' o 'Transformar'.
2.  En **AD FS administración**, haz clic en **confiar confía en las partes** y en el panel derecho, haga clic en el punto de reunión y selecciona **editar reglas de notificación**.
3.  En la **reglas de transformación de emisión** haga clic en **Agregar regla**.
4.  En la **transformar reclamación regla Asistente para agregar** selecciona **paso a través o una notificación entrante de filtro** y haz clic en **siguiente**.
5.  Agregar un nombre para mostrar y selecciona **grupo de dispositivos de Windows** desde el **tipo de notificación entrante** lista desplegable.
6.  Haz clic en **finalizar**.  Haz clic en **aplicar** y **Aceptar**.

## <a name="validation"></a>Validación
Para validar la versión de una reclamación de 'WindowsDeviceGroup', crear una prueba de las notificaciones de la aplicación compatible con .net 4.6. Con el SDK de WIF 4.0.
Configurar la aplicación como una fiesta confiar en ADFS y actualizarlo con Reclamación regla como se especifica en los pasos anteriores.
Al autenticar a la aplicación con el proveedor de autenticación integrada de Windows de ADFS, son convertir las siguientes notificaciones.
![Validación](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Ahora se pueden consumir las notificaciones para el dispositivo de equipo para los controles de acceso más enriquecidos.

Por ejemplo, el siguiente **AdditionalAuthenticationRules** indica AD FS para invocar MFA si: el usuario autenticación no es miembro del grupo de seguridad "-1-5-21-2134745077-1211275016-3050530490-1117" y el equipo (donde está el usuario está Autenticación de) no es miembro del grupo de seguridad "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

Sin embargo, si se cumplen algunas de las condiciones anteriores, invocar MFA.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```