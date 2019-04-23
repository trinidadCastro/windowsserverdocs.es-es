---
title: Configuración de control de acceso de usuario y permisos
description: Aprenda a configurar el control de acceso de usuario y permisos con Active Directory o Azure AD (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850576"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurar permisos y Control de acceso de usuario

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Si no lo ha hecho ya, familiarícese con el [opciones de control de acceso de usuario en Windows Admin Center](../plan/user-access-options.md)

>[!NOTE]
> Acceso de grupo basado en Windows Admin Center no se admite en entornos de grupo de trabajo o dominios que no son de confianza.

## <a name="gateway-access-role-definitions"></a>Definiciones de roles de acceso de puerta de enlace

Hay dos roles para el acceso al servicio de puerta de enlace de Windows Admin Center:

**Los usuarios de la puerta de enlace** puede conectarse al servicio de puerta de enlace de Windows Admin Center para administrar los servidores a través de esa puerta de enlace, pero no pueden cambiar los permisos de acceso ni el mecanismo de autenticación usada para autenticarse en la puerta de enlace.

**Los administradores de la puerta de enlace** puede configurar quién tiene acceso, así como la forma a los usuarios autentican en la puerta de enlace. Solo los administradores de la puerta de enlace pueden ver y configurar el acceso en Windows Admin Center. Los administradores locales en el equipo de puerta de enlace siempre son administradores del servicio de puerta de enlace de Windows Admin Center.

> [!NOTE]
> Acceso a la puerta de enlace no implica el acceso a los servidores administrados visible la puerta de enlace. Para administrar un servidor de destino, el usuario que se conecta debe utilizar credenciales (mediante sus credenciales de Windows se pasan o mediante las credenciales proporcionadas en la sesión de Windows Admin Center mediante el **administrar como** acción) que tienen acceso administrativo a ese servidor de destino.

## <a name="active-directory-or-local-machine-groups"></a>Active Directory o grupos de equipo local

De forma predeterminada, los grupos de equipo local o de Active Directory se usan para controlar el acceso de puerta de enlace. Si tiene un dominio de Active Directory, puede administrar el usuario de puerta de enlace y el Administrador de acceso desde dentro de la interfaz de Windows Admin Center.

En el **usuarios** ficha puede controlar quién puede acceder a Windows Admin Center como un usuario de la puerta de enlace. De forma predeterminada, y si no especifica un grupo de seguridad, cualquier usuario que tiene acceso a la dirección URL de puerta de enlace tenga acceso. Una vez que agregue uno o varios grupos de seguridad a la lista de usuarios, el acceso está restringido a los miembros de esos grupos.

Si no usa un dominio de Active Directory en su entorno, el acceso se controla mediante el ```Users``` y ```Administrators``` grupos locales en la máquina de puerta de enlace de Windows Admin Center.

### <a name="smartcard-authentication"></a>Autenticación de tarjeta inteligente

Puede aplicar **autenticación de tarjeta inteligente** especificando adicional _requiere_ para grupos de seguridad basada en tarjeta inteligente. Una vez haya agregado un grupo de seguridad basada en tarjeta inteligente, un usuario solo pueda acceder el servicio de Windows Admin Center si son miembros de cualquier grupo de seguridad y un grupo de tarjeta inteligente se incluye en la lista de usuarios.

En el **administradores** ficha puede controlar quién puede acceder a Windows Admin Center como un administrador de puerta de enlace. El grupo de administradores locales en el equipo siempre tendrá acceso de administrador completo y no se puede quitar de la lista. Mediante la adición de grupos de seguridad, asigne a los miembros de esos privilegios grupos para cambiar la configuración de puerta de enlace de Windows Admin Center. La lista de administradores admite la autenticación de tarjeta inteligente en la misma manera que la lista de usuarios: con la condición AND para un grupo de seguridad y un grupo de tarjeta inteligente.

## <a name="azure-active-directory"></a>Azure Active Directory

Si su organización usa Azure Active Directory (Azure AD), puede agregar un **adicionales** capa de seguridad para Windows Admin Center al requerir autenticación de Azure AD para tener acceso a la puerta de enlace. Para tener acceso a Windows Admin Center, el usuario **cuenta Windows** también debe tener acceso al servidor de puerta de enlace (incluso si se usa la autenticación de Azure AD). Cuando se usa Azure AD, podrá administrar los permisos de acceso de usuario y Administrador de Windows Admin Center desde el Portal de Azure, en lugar de desde la interfaz de usuario de Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Obtener acceso a Windows Admin Center cuando está habilitada la autenticación de Azure AD

Dependiendo del explorador que usa, algunos usuarios tienen acceso a Windows Admin Center con la autenticación de Azure AD configurada recibirán un símbolo del sistema adicional **desde el explorador** donde deben proporcionar sus credenciales de cuenta de Windows la máquina donde está instalado Windows Admin Center. Después de escribir esa información, los usuarios obtendrán el aviso de autenticación de Azure Active Directory adicional, que requiere las credenciales de una cuenta de Azure que se ha concedido acceso en la aplicación de Azure AD en Azure.

> [!NOTE]
> Los usuarios que Windows de la cuenta tiene **derechos de administrador** en la puerta de enlace máquina no se solicitará la autenticación de Azure AD.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configurar la autenticación de Azure Active Directory para Windows Admin Center Preview

Vaya a Windows Admin Center **configuración** > **acceso** y utilice el conmutador para activar "usar Azure Active Directory para agregar una capa de seguridad a la puerta de enlace". Si no ha registrado la puerta de enlace de Azure, se le guiará para hacerlo en este momento.

De forma predeterminada, todos los miembros del inquilino de Azure AD tienen acceso de usuario para el servicio de puerta de enlace de Windows Admin Center. Solo los administradores locales en el equipo de puerta de enlace tienen acceso de administrador a la puerta de enlace de Windows Admin Center. Tenga en cuenta que los derechos de administradores locales en la máquina de puerta de enlace no pueden ser restringidos, los administradores locales pueden hacer cualquier cosa, independientemente de si se usa Azure AD para la autenticación.

Si desea que Azure AD específico a los usuarios, los usuarios de la puerta de enlace de grupos o acceso de administrador de puerta de enlace para el servicio de Windows Admin Center, debe hacer lo siguiente:

1.  Vaya a la aplicación de Windows Admin Center de Azure AD en Azure portal mediante el hipervínculo proporcionado en la configuración de acceso. Tenga en cuenta que este hipervínculo solo está disponible cuando está habilitada la autenticación de Azure Active Directory. 
    -   También puede encontrar la aplicación en Azure portal, vaya a **Azure Active Directory** > **aplicaciones empresariales** > **detodaslasaplicaciones** y buscar **WindowsAdminCenter** (la aplicación de Azure AD se denominará WindowsAdminCenter -<gateway name>). Si no obtiene los resultados de búsqueda, asegúrese de **mostrar** está establecido en **todas las aplicaciones**, **estado de la aplicación** está establecido en **cualquier** y haga clic en aplicar, Vuelva a intentar la búsqueda. Una vez que haya encontrado la aplicación, vaya a **usuarios y grupos**
2.  En la pestaña Propiedades, establezca **asignación de usuarios necesaria** en Sí.
    Una vez hecho esto, solo los miembros se muestran en el **usuarios y grupos** ficha podrán tener acceso a la puerta de enlace de Windows Admin Center.
3.  En la pestaña usuarios y grupos, seleccione **Agregar usuario**. Debe asignar un usuario de la puerta de enlace o el rol de administrador de puerta de enlace para cada usuario o grupo agregado.

Después de activar la autenticación de Azure AD, se reinicia el servicio de puerta de enlace y se debe actualizar el explorador. Puede actualizar el acceso de usuario para la aplicación de PYME Azure AD en Azure portal en cualquier momento.

Los usuarios le pedirá que inicie sesión con su identidad de Azure Active Directory cuando intentan obtener acceso a la dirección URL de puerta de enlace de Windows Admin Center. Recuerde que los usuarios también deben ser un miembro de los usuarios locales en el servidor de puerta de enlace para tener acceso a Windows Admin Center.

Los usuarios y administradores pueden ver su cuenta ha iniciado sesión actualmente y, así como de cierre de esta cuenta de Azure AD desde el **cuenta** ficha de configuración de Windows Admin Center.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configurar la autenticación de Azure Active Directory para Windows Admin Center

[Para configurar la autenticación de Azure AD, primero debe registrar la puerta de enlace con Azure](azure-integration.md) (basta con hacerlo una vez para la puerta de enlace de Windows Admin Center). Este paso crea una aplicación de Azure AD desde el que puede administrar el acceso de administrador de puerta de enlace y de usuario de la puerta de enlace.

Si desea que Azure AD específico a los usuarios, los usuarios de la puerta de enlace de grupos o acceso de administrador de puerta de enlace para el servicio de Windows Admin Center, debe hacer lo siguiente:

1.  Vaya a la aplicación de PYME Azure AD en Azure portal. 
    -   Al hacer clic en **Cambiar control de acceso** y, a continuación, seleccione **Azure Active Directory** desde la configuración de acceso de Windows Admin Center, puede usar el hipervínculo proporcionado en la interfaz de usuario para tener acceso a Azure AD aplicación en Azure portal. Este hipervínculo también está disponible en la configuración de acceso después de que haga clic en Guardar y ha seleccionado Azure AD como proveedor de identidades de control de acceso.
    -   También puede encontrar la aplicación en Azure portal, vaya a **Azure Active Directory** > **aplicaciones empresariales** > **detodaslasaplicaciones** y buscar **SME** (la aplicación de Azure AD se denominará SME -<gateway>). Si no obtiene los resultados de búsqueda, asegúrese de **mostrar** está establecido en **todas las aplicaciones**, **estado de la aplicación** está establecido en **cualquier** y haga clic en aplicar, Vuelva a intentar la búsqueda. Una vez que haya encontrado la aplicación, vaya a **usuarios y grupos**
2.  En la pestaña Propiedades, establezca **asignación de usuarios necesaria** en Sí.
    Una vez hecho esto, solo los miembros se muestran en el **usuarios y grupos** ficha podrán tener acceso a la puerta de enlace de Windows Admin Center.
3.  En la pestaña usuarios y grupos, seleccione **Agregar usuario**. Debe asignar un usuario de la puerta de enlace o el rol de administrador de puerta de enlace para cada usuario o grupo agregado.

Una vez que guarde el de Azure AD control de acceso en el **Cambiar control de acceso** se reinicia el servicio de puerta de enlace de panel, y debe actualizar el explorador. Puede actualizar el acceso de usuario para la aplicación de Windows Admin Center de Azure AD en Azure portal en cualquier momento. 

Los usuarios le pedirá que inicie sesión con su identidad de Azure Active Directory cuando intentan obtener acceso a la dirección URL de puerta de enlace de Windows Admin Center. Recuerde que los usuarios también deben ser un miembro de los usuarios locales en el servidor de puerta de enlace para tener acceso a Windows Admin Center. 

Mediante el **Azure** pestaña de configuración general de Windows Admin Center, los usuarios y administradores puede ver su cuenta ha iniciado sesión actualmente y, así como de cierre de esta cuenta de Azure AD.

### <a name="conditional-access-and-multi-factor-authentication"></a>Acceso condicional y Multi-factor authentication

Una de las ventajas de usar Azure AD como una capa adicional de seguridad para controlar el acceso a la puerta de enlace de Windows Admin Center es que puede aprovechar las características de seguridad muy eficaces de Azure AD como el acceso condicional y la autenticación multifactor. 

[Más información sobre cómo configurar el acceso condicional con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurar el inicio de sesión único

**De sesión único cuando se implementa como un servicio de Windows Server**

Cuando se instala Windows Admin Center en Windows 10, está listo para usar el inicio de sesión único. Sin embargo, si va a usar Windows Admin Center en Windows Server, deberá configurar algún tipo de delegación de Kerberos en su entorno antes de poder usar el inicio de sesión único. La delegación configura el equipo de puerta de enlace como de confianza para delegar en el nodo de destino. 

Para configurar [la delegación restringida basada en recursos](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1) en su entorno, ejecute los siguientes cmdlets de PowerShell. (Puede tener en cuenta que esto requiere un controlador de dominio que ejecutan Windows Server 2012 o posterior).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

En este ejemplo, la puerta de enlace de Windows Admin Center está instalado en servidor **WindowsAdminCenterGW**, y el nombre de nodo de destino es **ManagedNode**.

Para quitar esta relación, ejecute el siguiente cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Control de acceso basado en roles

Control de acceso basado en roles permite proporcionar a los usuarios con acceso limitado a la máquina en lugar de realizar los administradores locales completa de ellos.
[Más información sobre el control de acceso basado en roles y los roles disponibles.](../plan/user-access-options.md#role-based-access-control)

Configuración de RBAC consta de 2 pasos: habilitar la compatibilidad en los equipos de destino y asignar usuarios a los roles pertinentes.

> [!TIP]
> Asegúrese de que tener privilegios de administrador local en los equipos donde va a configurar la compatibilidad con el control de acceso basado en roles.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Control de acceso basado en roles se aplican a un único equipo

El modelo de implementación de máquina única es ideal para entornos simples con solo unos pocos equipos para administrar.
Configuración de una máquina con compatibilidad para control de acceso basado en roles dará como resultado los siguientes cambios:
-   Módulos de PowerShell con las funciones requeridas por Windows Admin Center se instalará en la unidad del sistema, en `C:\Program Files\WindowsPowerShell\Modules`. Todos los módulos se iniciarán con **Microsoft.Sme**
-   Desired State Configuration se ejecutará una única configuración para configurar un punto de conexión de Just Enough Administration en la máquina, denominada **Microsoft.Sme.PowerShell**. Este punto de conexión define los 3 roles usando Windows Admin Center y se ejecutará como administrador local temporal cuando un usuario se conecta a él.
-   se crearán 3 nuevos grupos locales a los usuarios que tengan concedidos acceso a los roles de control:
    -   Administradores de Windows Admin Center
    -   Administradores de Hyper-V de Windows Admin Center
    -   Windows Admin Center Readers

Para habilitar la compatibilidad con el control de acceso basado en roles en un solo equipo, siga estos pasos:

1.  Abra Windows Admin Center y conéctese a la máquina que desea configurar con el control de acceso basado en rol con una cuenta con privilegios de administrador local en el equipo de destino.
2.  En el **Introducción** de herramientas, haga clic en **configuración** > **control de acceso basado en roles**.
3.  Haga clic en **aplicar** en la parte inferior de la página para habilitar la compatibilidad con control de acceso basado en roles en el equipo de destino. El proceso de aplicación implica copiar los scripts de PowerShell y la llamada a una configuración (con Desired State Configuration de PowerShell) en el equipo de destino. Puede tardar hasta 10 minutos en completarse y dará como resultado reiniciar WinRM. Esto desconectará temporalmente a los usuarios de Windows Admin Center, PowerShell y WMI.
4.  Actualice la página para comprobar el estado de control de acceso basado en roles. Cuando esté listo para su uso, el estado cambiará a **aplicado**.

Una vez que se aplica la configuración, puede asignar a usuarios a los roles:

1.  Abra el **usuarios y grupos locales** herramienta y vaya a la **grupos** ficha.
2.  Seleccione el **Windows Admin Center lectores** grupo.
3.  En el *detalles* panel en la parte inferior, haga clic en **Agregar usuario** y escriba el nombre de un usuario o grupo de seguridad que debe tener acceso de solo lectura en el servidor a través de Windows Admin Center. Los usuarios y grupos pueden proceder de la máquina local o dominio de Active Directory.
4.  Repita los pasos 2 y 3 para el **administradores de Hyper-V de Windows Admin Center** y **Windows Admin Center administradores** grupos.

También puede rellenar estos grupos de forma coherente a través de su dominio mediante la configuración de un objeto de directiva de grupo con el [configuración de directiva de grupos restringidos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Control de acceso basado en roles se aplican a varias máquinas

En una implementación de empresa de gran tamaño, puede usar las herramientas de automatización existente para insertar la característica de control de acceso basado en roles en los equipos mediante la descarga del paquete de configuración de la puerta de enlace de Windows Admin Center.
El paquete de configuración está diseñado para usarse con Desired State Configuration de PowerShell, pero puede adaptar para que funcione con la solución de automatización preferido.

#### <a name="download-the-role-based-access-control-configuration"></a>Descargar la configuración de control de acceso basado en roles

Para descargar el paquete de configuración de control de acceso basado en roles, necesita tener acceso a Windows Admin Center y un símbolo del sistema de PowerShell.

Si está ejecutando la puerta de enlace de Windows Admin Center en modo de servicio en Windows Server, use el comando siguiente para descargar el paquete de configuración.
Asegúrese de actualizar la dirección de puerta de enlace con el correcto para su entorno.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Si está ejecutando la puerta de enlace de Windows Admin Center en el equipo de Windows 10, ejecute el siguiente comando:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Al expandir el archivo zip, verá la siguiente estructura de carpetas:
- InstallJeaFeatures.ps1
- JustEnoughAdministration (directorio)
- Módulos (directorio)
    - Microsoft.SME. \* (directorios)
    - WindowsAdminCenter.Jea (directory)

Para configurar la compatibilidad con el control de acceso basado en roles en un nodo, debe realizar las siguientes acciones:
1.  Copie el JustEnoughAdministration, Microsoft.SME. \*y los módulos de WindowsAdminCenter.Jea en el directorio de módulo de PowerShell en el equipo de destino. Normalmente, esto se encuentra en `C:\Program Files\WindowsPowerShell\Modules`.
2.  Actualización **InstallJeaFeature.ps1** archivo para que coincida con la configuración deseada para el punto de conexión RBAC.
3.  Ejecute InstallJeaFeature.ps1 para compilar el recurso de DSC.
4.  Implementar la configuración de DSC en todas las máquinas para aplicar la configuración.

La siguiente sección explica cómo hacer esto mediante la comunicación remota de PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Implementar en varios equipos

Para implementar la configuración que descargó en varios equipos, deberá actualizar el **InstallJeaFeatures.ps1** script para incluir los grupos de seguridad adecuados para su entorno, copie los archivos en cada uno de los equipos, e invocar los scripts de configuración.
Puede usar las herramientas de automatización preferida para lograr esto, sin embargo, en este artículo se centrará en un enfoque puro basada en PowerShell.

De forma predeterminada, el script de configuración creará grupos de seguridad locales en el equipo para controlar el acceso a cada uno de los roles.
Esto es adecuado para el grupo de trabajo y equipos unidos a dominio, pero si va a implementar en un entorno de dominio de sólo puede desear directamente asociar un grupo de seguridad de dominio a cada rol.
Para actualizar la configuración para usar grupos de seguridad de dominio, abra **InstallJeaFeatures.ps1** y realice los cambios siguientes:

1.  Quitar los 3 **grupo** recursos desde el archivo:
    1.  "Grupo de lectores de MS de grupo"
    2.  "Grupo de MS-Hyper-V-administradores-Group"
    3.  "Grupo de administradores de MS de grupo"
2.  Quitar los recursos del grupo 3 de la JeaEndpoint **DependsOn** propiedad
    1.  "[Group] grupo de lectores de MS"
    2.  "[Group]MS-Hyper-V-Administrators-Group"
    3.  "[Group] grupo de administradores de MS"
3.  Cambiar los nombres de grupo en el JeaEndpoint **RoleDefinitions** propiedad a los grupos de seguridad deseado. Por ejemplo, si tiene un grupo de seguridad *CONTOSO\MyTrustedAdmins* que debe asignarse acceso a la función de los administradores de Windows Admin Center, cambio `'$env:COMPUTERNAME\Windows Admin Center Administrators'` a `'CONTOSO\MyTrustedAdmins'`. Las tres cadenas que deba actualizar son:
    1.  '$env:COMPUTERNAME\Windows Admin Center Administrators'
    2.  '$env:COMPUTERNAME\Windows Admin Center Hyper-V Administrators'
    3.  '$env:COMPUTERNAME\Windows Admin Center Readers'

> [!NOTE]
> Asegúrese de usar grupos de seguridad únicos para cada rol. Se producirá un error en la configuración si se asigna el mismo grupo de seguridad a varios roles.

A continuación, al final de la **InstallJeaFeatures.ps1** , agregue las siguientes líneas de PowerShell en la parte inferior de la secuencia de comandos:

```powershell
Copy-Item "$PSScriptRoot\JustEnoughAdministration" "$env:ProgramFiles\WindowsPowerShell\Modules" -Recurse -Force
$ConfigData = @{
    AllNodes = @()
    ModuleBasePath = @{
        Source = "$PSScriptRoot\Modules"
        Destination = "$env:ProgramFiles\WindowsPowerShell\Modules"
    }
}
InstallJeaFeature -ConfigurationData $ConfigData | Out-Null
Start-DscConfiguration -Path "$PSScriptRoot\InstallJeaFeature" -JobName "Installing JEA for Windows Admin Center" -Force
```

Por último, puede copiar la carpeta que contiene los módulos, recursos de DSC y configuración para cada nodo de destino y ejecute el **InstallJeaFeature.ps1** secuencia de comandos.
Para hacerlo de forma remota desde su estación de trabajo de administrador, puede ejecutar los comandos siguientes:

```powershell
$ComputersToConfigure = 'MyServer01', 'MyServer02'

$ComputersToConfigure | ForEach-Object {
    $session = New-PSSession -ComputerName $_ -ErrorAction Stop
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC\JustEnoughAdministration\" -Destination "$env:ProgramFiles\WindowsPowerShell\Modules\" -ToSession $session -Recurse -Force
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC" -Destination "$env:TEMP\WindowsAdminCenter_RBAC" -ToSession $session -Recurse -Force
    Invoke-Command -Session $session -ScriptBlock { Import-Module JustEnoughAdministration; & "$env:TEMP\WindowsAdminCenter_RBAC\InstallJeaFeature.ps1" } -AsJob
    Disconnect-PSSession $session
}
```
