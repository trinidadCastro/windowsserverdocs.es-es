---
title: Autenticación compuesta y notificaciones de Active Directory Domain Services en Servicios de federación de Active Directory (AD FS)
description: En el siguiente documento se describe la autenticación compuesta y las notificaciones de AD DS en AD FS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e02ce6400bc9905814e6ad7dcf02614c0dff5e46
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965177"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Autenticación compuesta y notificaciones de AD DS en AD FS
Windows Server 2012 mejora la autenticación Kerberos mediante la introducción de la autenticación compuesta.  La autenticación compuesta permite a una solicitud del servicio de concesión de vales (TGS) de Kerberos incluir dos identidades: 

- la identidad del usuario
- identidad del dispositivo del usuario.  

Windows consigue la autenticación compuesta mediante la extensión de la protección de Kerberos de [Autenticación flexible (Fast) o la protección de Kerberos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)). 

AD FS 2012 y las versiones posteriores permiten el consumo de AD DS notificaciones de usuario o de dispositivo emitidas que residen en un vale de autenticación Kerberos. En versiones anteriores de AD FS, el motor de notificaciones solo podía leer los identificadores de seguridad (SID) de usuarios y grupos de Kerberos, pero no podía leer ninguna información de notificaciones contenida dentro de un vale de Kerberos.

Puede habilitar el control de acceso más completo para aplicaciones federadas mediante el uso conjunto de Active Directory Domain Services (AD DS) y notificaciones de dispositivo, con Servicios de federación de Active Directory (AD FS) (AD FS).

## <a name="requirements"></a>Requisitos
1.  Los equipos que tienen acceso a las aplicaciones federadas deben autenticarse en AD FS mediante la **autenticación integrada de Windows**. 
    - La autenticación integrada de Windows solo está disponible cuando se conecta a los servidores back-end AD FS.
    - Los equipos deben poder tener acceso a los servidores de AD FS de back-end para Servicio de federación nombre
    - Los servidores AD FS deben ofrecer la autenticación integrada de Windows como método de autenticación principal en la configuración de la intranet.

2.  La directiva **de compatibilidad del cliente Kerberos con la autenticación compuesta de notificaciones y la protección de Kerberos** debe aplicarse a todos los equipos que tengan acceso a las aplicaciones federadas protegidas por la autenticación compuesta. Esto es aplicable en caso de escenarios de bosque único o entre bosques.

3.  El dominio que aloja los servidores de AD FS debe tener la configuración **de Directiva compatibilidad con KDC para autenticación compuesta de notificaciones y protección de Kerberos** aplicada a los controladores de dominio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Pasos para configurar AD FS en Windows Server 2012 R2
Siga estos pasos para configurar la autenticación compuesta y las notificaciones 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Paso 1: habilitar la compatibilidad con KDC para notificaciones, autenticación compuesta y protección de Kerberos en la directiva predeterminada de controladores de dominio
1.  En Administrador del servidor, seleccione Herramientas, **Administración de directiva de grupo**.
2.  Desplácese hacia abajo hasta la **directiva predeterminada de controladores de dominio**, haga clic con el botón derecho y seleccione **Editar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  En el **Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **sistema**y, a continuación, seleccione **KDC**.
4.  En el panel derecho, haga doble clic en **compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  En la ventana nuevo cuadro de diálogo, establezca compatibilidad de KDC con notificaciones **habilitadas**.
6.  En opciones, seleccione **compatible** en el menú desplegable y, a continuación, haga clic en **aplicar** y en **Aceptar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Paso 2: habilitar la compatibilidad del cliente Kerberos con notificaciones, autenticación compuesta y protección de Kerberos en equipos que acceden a aplicaciones federadas

1.  En un directiva de grupo aplicado a los equipos que acceden a las aplicaciones federadas, en el **Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **sistema**y seleccione **Kerberos**.
2.  En el panel derecho de la ventana de Editor de administración de directivas de grupo, haga doble clic en **compatibilidad del cliente Kerberos con notificaciones, autenticación compuesta y protección de Kerberos.**
3.  En la ventana nuevo cuadro de diálogo, establezca compatibilidad del cliente Kerberos en **habilitado** y haga clic en **aplicar** y en **Aceptar**.
![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Cierre el Editor de administración de directivas de grupo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Paso 3: Asegúrese de que se han actualizado los servidores de AD FS.
Debe asegurarse de que las siguientes actualizaciones estén instaladas en los servidores de AD FS.

|Actualización|Descripción|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Actualización de seguridad acumulativa (incluye KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Actualización para Server 2012 R2|
|[Revisión 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Esta actualización agrega compatibilidad con las notificaciones de identificador compuesto en Servicios de federación de Active Directory (AD FS).|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Paso 4: configurar el proveedor de autenticación principal

1. Establezca el proveedor de autenticación principal en **autenticación de Windows** para AD FS la configuración de la intranet.
2. En administración de AD FS, **en directivas de autenticación**, seleccione **autenticación principal** y, en **configuración global** , haga clic en **Editar**.
3. En **Editar Directiva de autenticación global** , en **intranet** , seleccione **autenticación de Windows**.
4. Haga clic en **aplicar** y en **Aceptar**.

![Administración de directivas de grupo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Con PowerShell puede usar el cmdlet **set-AdfsGlobalAuthenticationPolicy** .

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>En una granja basada en WID, el comando de PowerShell se debe ejecutar en el servidor de AD FS principal.
>En una granja basada en SQL, el comando de PowerShell se puede ejecutar en cualquier servidor AD FS que sea miembro de la granja.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Paso 5: agregar la descripción de la demanda a AD FS
1. Agregue la siguiente descripción de la declaración a la granja. Esta descripción de notificaciones no está presente de forma predeterminada en ADFS 2012 R2 y debe agregarse manualmente.
2. En administración de AD FS, en **servicio**, haga clic con el botón derecho en Descripción de la **reclamación** y seleccione **Agregar Descripción** de la reclamación.
3. Escriba la siguiente información en la descripción de la demanda.
   - Nombre para mostrar: ' grupo de dispositivos de Windows ' 
   - Descripción de la demanda: ' <https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup> ' '
4. Active ambas casillas.
5. Haga clic en **OK**.

![Descripción de la demanda](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Con PowerShell puede usar el cmdlet **Add-AdfsClaimDescription** .
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
   ```


>[!NOTE]
>En una granja basada en WID, el comando de PowerShell se debe ejecutar en el servidor de AD FS principal.
>En una granja basada en SQL, el comando de PowerShell se puede ejecutar en cualquier servidor AD FS que sea miembro de la granja.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Paso 6: habilitación del bit de autenticación compuesta en el atributo msDS-SupportedEncryptionTypes

1.  Habilite el bit de autenticación compuesta en el atributo msDS-SupportedEncryptionTypes de la cuenta que designó para ejecutar el servicio de AD FS mediante el cmdlet **de PowerShell Set-ADServiceAccount** .  

>[!NOTE]
>Si cambia la cuenta de servicio, debe habilitar manualmente la autenticación compuesta mediante la ejecución de los cmdlets de Windows PowerShell **set-ADUser-compoundIdentitySupported: $true** .

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Reinicie el servicio ADFS.

>[!NOTE]
>Una vez que ' CompoundIdentitySupported ' está establecido en true, se produce el siguiente error en la instalación del mismo gMSA en los servidores nuevos (2012R2/2016) con el siguiente error: **install-ADServiceAccount: no se puede instalar la cuenta de servicio. Mensaje de error: "el contexto proporcionado no coincidía con el destino."**.
>
>**Solución**: establezca temporalmente CompoundIdentitySupported en $false. Este paso hace que ADFS detenga la emisión de notificaciones de WindowsDeviceGroup. Set-ADServiceAccount-Identity ' cuenta del servicio ADFS '-CompoundIdentitySupported: $false Instale el gMSA en el nuevo servidor y, a continuación, habilite CompoundIdentitySupported en $True.
Deshabilitar CompoundIdentitySupported y, a continuación, volver a habilitar no necesita reiniciar el servicio AD FS.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Paso 7: actualizar la AD FS confianza del proveedor de notificaciones para Active Directory

1. Actualice la AD FS confianza del proveedor de notificaciones para que Active Directory incluya la siguiente regla de notificación de paso a través para la notificación ' WindowsDeviceGroup '.
2.  En **Administración de AD FS**, haga clic en **confianzas de proveedor de notificaciones** y, en el panel derecho, haga clic en aparece **Active Directory** y seleccione **editar reglas de notificación**.
3.  En **editar reglas de notificaciones para el director activo** , haga clic en **Agregar regla**.
4.  En el **Asistente para agregar regla de notificaciones de transformación** , seleccione **pasar a través o filtrar una notificaciones entrantes** y haga clic en **siguiente**.
5.  Agregue un nombre para mostrar y seleccione **grupo de dispositivos de Windows** en la lista desplegable tipo de **notificaciones entrantes** .
6.  Haga clic en **Finalizar**  Haga clic en **aplicar** y en **Aceptar**. 
![Descripción de la demanda](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Paso 8: en el usuario de confianza en el que se esperan las notificaciones ' WindowsDeviceGroup ', agregue una regla de notificación similar de ' paso a través ' o ' transformación '.
2. En **Administración de AD FS**, haga clic en relaciones de confianza para usuario **autenticado** y, en el panel derecho, haga clic en el RP aparece y seleccione **editar reglas de notificaciones**.
3. En **reglas de transformación de emisión** , haga clic en **Agregar regla**.
4. En el **Asistente para agregar regla de notificaciones de transformación** , seleccione **pasar a través o filtrar una notificaciones entrantes** y haga clic en **siguiente**.
5. Agregue un nombre para mostrar y seleccione **grupo de dispositivos de Windows** en la lista desplegable tipo de **notificaciones entrantes** .
6. Haga clic en **Finalizar**  Haga clic en **aplicar** y en **Aceptar**.
   ![Descripción de la demanda](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Pasos para configurar AD FS en Windows Server 2016
A continuación se detallan los pasos para configurar la autenticación compuesta en AD FS para Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Paso 1: habilitar la compatibilidad con KDC para notificaciones, autenticación compuesta y protección de Kerberos en la directiva predeterminada de controladores de dominio
1.  En Administrador del servidor, seleccione Herramientas, **Administración de directiva de grupo**.
2.  Desplácese hacia abajo hasta la **directiva predeterminada de controladores de dominio**, haga clic con el botón derecho y seleccione **Editar**.
3.  En el **Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **sistema**y, a continuación, seleccione **KDC**.
4.  En el panel derecho, haga doble clic en **compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos**.
5.  En la ventana nuevo cuadro de diálogo, establezca compatibilidad de KDC con notificaciones **habilitadas**.
6.  En opciones, seleccione **compatible** en el menú desplegable y, a continuación, haga clic en **aplicar** y en **Aceptar**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Paso 2: habilitar la compatibilidad del cliente Kerberos con notificaciones, autenticación compuesta y protección de Kerberos en equipos que acceden a aplicaciones federadas

1.  En un directiva de grupo aplicado a los equipos que acceden a las aplicaciones federadas, en el **Editor de administración de directivas de grupo**, en **configuración del equipo**, expanda **directivas**, expanda **plantillas administrativas**, expanda **sistema**y seleccione **Kerberos**.
2.  En el panel derecho de la ventana de Editor de administración de directivas de grupo, haga doble clic en **compatibilidad del cliente Kerberos con notificaciones, autenticación compuesta y protección de Kerberos.**
3.  En la ventana nuevo cuadro de diálogo, establezca compatibilidad del cliente Kerberos en **habilitado** y haga clic en **aplicar** y en **Aceptar**.
4.  Cierre el Editor de administración de directivas de grupo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Paso 3: configurar el proveedor de autenticación principal

1. Establezca el proveedor de autenticación principal en **autenticación de Windows** para AD FS la configuración de la intranet.
2. En administración de AD FS, **en directivas de autenticación**, seleccione **autenticación principal** y, en **configuración global** , haga clic en **Editar**.
3. En **Editar Directiva de autenticación global** , en **intranet** , seleccione **autenticación de Windows**.
4. Haga clic en **aplicar** y en **Aceptar**.
5. Con PowerShell puede usar el cmdlet **set-AdfsGlobalAuthenticationPolicy** .

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>En una granja basada en WID, el comando de PowerShell se debe ejecutar en el servidor de AD FS principal.
>En una granja basada en SQL, el comando de PowerShell se puede ejecutar en cualquier servidor AD FS que sea miembro de la granja.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Paso 4: habilitación del bit de autenticación compuesta en el atributo msDS-SupportedEncryptionTypes

1.  Habilite el bit de autenticación compuesta en el atributo msDS-SupportedEncryptionTypes de la cuenta que designó para ejecutar el servicio de AD FS mediante el cmdlet **de PowerShell Set-ADServiceAccount** .  

>[!NOTE]
>Si cambia la cuenta de servicio, debe habilitar manualmente la autenticación compuesta mediante la ejecución de los cmdlets de Windows PowerShell **set-ADUser-compoundIdentitySupported: $true** .

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Reinicie el servicio ADFS.

>[!NOTE]
>Una vez que ' CompoundIdentitySupported ' está establecido en true, se produce el siguiente error en la instalación del mismo gMSA en los servidores nuevos (2012R2/2016) con el siguiente error: **install-ADServiceAccount: no se puede instalar la cuenta de servicio. Mensaje de error: "el contexto proporcionado no coincidía con el destino."**.
>
>**Solución**: establezca temporalmente CompoundIdentitySupported en $false. Este paso hace que ADFS detenga la emisión de notificaciones de WindowsDeviceGroup. Set-ADServiceAccount-Identity ' cuenta del servicio ADFS '-CompoundIdentitySupported: $false Instale el gMSA en el nuevo servidor y, a continuación, habilite CompoundIdentitySupported en $True.
Deshabilitar CompoundIdentitySupported y, a continuación, volver a habilitar no necesita reiniciar el servicio AD FS.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Paso 5: actualización de la confianza del proveedor de notificaciones de AD FS para Active Directory

1. Actualice la AD FS confianza del proveedor de notificaciones para que Active Directory incluya la siguiente regla de notificación de paso a través para la notificación ' WindowsDeviceGroup '.
2.  En **Administración de AD FS**, haga clic en **confianzas de proveedor de notificaciones** y, en el panel derecho, haga clic en aparece **Active Directory** y seleccione **editar reglas de notificación**.
3.  En **editar reglas de notificaciones para el director activo** , haga clic en **Agregar regla**.
4.  En el **Asistente para agregar regla de notificaciones de transformación** , seleccione **pasar a través o filtrar una notificaciones entrantes** y haga clic en **siguiente**.
5.  Agregue un nombre para mostrar y seleccione **grupo de dispositivos de Windows** en la lista desplegable tipo de **notificaciones entrantes** .
6.  Haga clic en **Finalizar**  Haga clic en **aplicar** y en **Aceptar**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Paso 6: en el usuario de confianza en el que se esperan las notificaciones ' WindowsDeviceGroup ', agregue una regla de notificación similar de ' paso a través ' o ' transformación '.
2. En **Administración de AD FS**, haga clic en relaciones de confianza para usuario **autenticado** y, en el panel derecho, haga clic en el RP aparece y seleccione **editar reglas de notificaciones**.
3. En **reglas de transformación de emisión** , haga clic en **Agregar regla**.
4. En el **Asistente para agregar regla de notificaciones de transformación** , seleccione **pasar a través o filtrar una notificaciones entrantes** y haga clic en **siguiente**.
5. Agregue un nombre para mostrar y seleccione **grupo de dispositivos de Windows** en la lista desplegable tipo de **notificaciones entrantes** .
6. Haga clic en **Finalizar**  Haga clic en **aplicar** y en **Aceptar**.

## <a name="validation"></a>Validación
Para validar la liberación de notificaciones de ' WindowsDeviceGroup ', cree una aplicación compatible con notificaciones de prueba mediante .net 4,6. Con WIF SDK 4,0.
Configure la aplicación como un usuario de confianza en ADFS y actualícelo con la regla de notificaciones como se especifica en los pasos anteriores.
Al autenticarse en la aplicación mediante el proveedor de autenticación integrada de Windows de ADFS, se convierten las siguientes notificaciones.
![Validación](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Las notificaciones para el equipo o dispositivo ahora pueden usarse para controles de acceso más completos.

Por ejemplo, el siguiente **AdditionalAuthenticationRules** indica AD FS para invocar MFA si: el usuario que realiza la autenticación no es miembro del grupo de seguridad "-1-5-21-2134745077-1211275016-3050530490-1117" y el equipo (desde el que el usuario se está autenticando) no es miembro del grupo de seguridad "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)".

Sin embargo, si se cumple alguna de las condiciones anteriores, no invoque MFA.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
