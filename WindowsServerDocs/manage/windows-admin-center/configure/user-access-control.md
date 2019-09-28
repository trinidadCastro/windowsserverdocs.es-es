---
title: Configuración del control de acceso y los permisos de usuario
description: Aprenda a configurar el control de acceso y los permisos de usuario con Active Directory o Azure AD (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 20b311e9330880c2b26e2494aabe27bb04891868
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407033"
---
# <a name="configure-user-access-control-and-permissions"></a>Configuración de Access Control de usuario y permisos

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Si todavía no lo ha hecho, familiarícese con las [Opciones de control de acceso de usuario en el centro de administración de Windows](../plan/user-access-options.md)

> [!NOTE]
> El acceso basado en grupo del centro de administración de Windows no se admite en entornos de grupo de trabajo o en dominios que no son de confianza.

## <a name="gateway-access-role-definitions"></a>Definiciones de roles de acceso de puerta de enlace

Hay dos roles para tener acceso al servicio de puerta de enlace del centro de administración de Windows:

**Los usuarios de puerta de enlace** pueden conectarse al servicio de puerta de enlace del centro de administración de Windows para administrar los servidores a través de esa puerta de enlace, pero no pueden cambiar los permisos de acceso ni el mecanismo de autenticación usado para autenticarse en la puerta de enlace.

**Los administradores de puerta de enlace** pueden configurar quién obtiene el acceso y cómo se autentican los usuarios en la puerta de enlace. Solo los administradores de puerta de enlace pueden ver y configurar la configuración de acceso en el centro de administración de Windows. Los administradores locales en el equipo de puerta de enlace son siempre administradores del servicio de puerta de enlace del centro de administración de Windows.

> [!NOTE]
> El acceso a la puerta de enlace no implica el acceso a los servidores administrados visibles para la puerta de enlace. Para administrar un servidor de destino, el usuario que se conecta debe usar las credenciales (ya sea a través de la credencial de Windows pasada a través o a través de las credenciales proporcionadas en la sesión del centro de administración de Windows mediante la acción **administrar como** ) que tienen acceso administrativo a ese servidor de destino.

## <a name="active-directory-or-local-machine-groups"></a>Active Directory o grupos de máquinas locales

De forma predeterminada, Active Directory o grupos de máquinas locales se utilizan para controlar el acceso a la puerta de enlace. Si tiene un dominio Active Directory, puede administrar el acceso de usuario y administrador de puerta de enlace desde la interfaz del centro de administración de Windows.

En la pestaña **usuarios** puede controlar quién puede tener acceso al centro de administración de Windows como usuario de puerta de enlace. De forma predeterminada, y si no especifica un grupo de seguridad, cualquier usuario que tenga acceso a la dirección URL de la puerta de enlace tiene acceso. Una vez que se agregan uno o varios grupos de seguridad a la lista de usuarios, el acceso está restringido a los miembros de esos grupos.

Si no usa un dominio Active Directory en su entorno, el acceso se controla mediante los grupos locales `Users` y `Administrators` en el equipo de puerta de enlace del centro de administración de Windows.

### <a name="smartcard-authentication"></a>Autenticación mediante tarjeta inteligente

Puede exigir la **autenticación mediante tarjeta inteligente** especificando un grupo adicional _obligatorio_ para los grupos de seguridad basados en tarjeta inteligente. Una vez agregado un grupo de seguridad basado en tarjeta inteligente, un usuario solo puede tener acceso al servicio del centro de administración de Windows si es miembro de cualquier grupo de seguridad y un grupo de tarjetas inteligentes incluido en la lista de usuarios.

En la pestaña **administradores** puede controlar quién puede tener acceso al centro de administración de Windows como administrador de puerta de enlace. El grupo de administradores locales en el equipo siempre tendrá acceso de administrador completo y no se puede quitar de la lista. Al agregar grupos de seguridad, concede a los miembros de esos grupos privilegios para cambiar la configuración de la puerta de enlace del centro de administración de Windows. La lista administradores admite la autenticación mediante tarjeta inteligente de la misma forma que la lista de usuarios: con las condiciones y para un grupo de seguridad y un grupo de tarjetas inteligentes.

## <a name="azure-active-directory"></a>Azure Active Directory

Si su organización usa Azure Active Directory (Azure AD), puede optar por agregar una capa **adicional** de seguridad al centro de administración de Windows al requerir la autenticación de Azure ad para tener acceso a la puerta de enlace. Para obtener acceso al centro de administración de Windows, la **cuenta de Windows** del usuario también debe tener acceso al servidor de puerta de enlace (incluso si se usa la autenticación Azure ad). Al usar Azure AD, podrá administrar los permisos de acceso de usuario y administrador del centro de administración de Windows desde el portal de Azure, en lugar de desde la interfaz de usuario del centro de administración de Windows.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Acceso al centro de administración de Windows cuando está habilitada la autenticación de Azure AD

En función del explorador que se use, algunos usuarios que accedan al centro de administración de Windows con la autenticación de Azure AD configurada recibirán un aviso adicional **desde el explorador** , donde deben proporcionar sus credenciales de cuenta de Windows para el equipo en el que El centro de administración de Windows está instalado. Después de escribir esa información, los usuarios obtendrán la solicitud de autenticación Azure Active Directory adicional, que requiere las credenciales de una cuenta de Azure a la que se haya concedido acceso en la aplicación Azure AD en Azure.

> [!NOTE]
> Los usuarios que tengan una cuenta de Windows con **derechos de administrador** en la máquina de puerta de enlace no se les solicitará la autenticación de Azure ad.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configuración de la autenticación de Azure Active Directory para la versión preliminar del centro de administración de Windows

Vaya a **configuración**del centro de administración de Windows  > **Access** y use el modificador para activar "usar Azure Active Directory para agregar una capa de seguridad a la puerta de enlace". Si no ha registrado la puerta de enlace en Azure, se le guiará para que lo haga en este momento.

De forma predeterminada, todos los miembros del inquilino Azure AD tienen acceso de usuario al servicio de puerta de enlace del centro de administración de Windows. Solo los administradores locales en el equipo de puerta de enlace tienen acceso de administrador a la puerta de enlace del centro de administración de Windows. Tenga en cuenta que los derechos de los administradores locales en el equipo de la puerta de enlace no pueden ser restringidos: los administradores locales pueden hacer nada independientemente de si se usa Azure AD para la autenticación.

Si desea conceder acceso específico a usuarios de Azure AD o administradores de puerta de enlace de usuarios o grupos al servicio del centro de administración de Windows, debe hacer lo siguiente:

1.  Vaya al centro de administración de Windows Azure AD aplicación en el Azure Portal mediante el hipervínculo proporcionado en configuración de acceso. Nota: este hipervínculo solo está disponible cuando está habilitada la autenticación de Azure Active Directory. 
    -   También puede encontrar la aplicación en el Azure Portal si va a **Azure Active Directory** > **Enterprise Applications** > **todas las aplicaciones** y busca **WindowsAdminCenter** (la aplicación Azure ad se denominará WindowsAdminCenter-<gateway name>). Si no obtiene ningún resultado de búsqueda, asegúrese de que **Mostrar** está establecido en **todas las aplicaciones**, el estado de la **aplicación** está establecido en **cualquiera** y haga clic en aplicar y, a continuación, intente realizar la búsqueda. Una vez que haya encontrado la aplicación, vaya a **usuarios y grupos**
2.  En la pestaña propiedades, establezca **asignación de usuario requerida** en sí.
    Una vez hecho esto, solo los miembros que aparecen en la pestaña **usuarios y grupos** podrán acceder a la puerta de enlace del centro de administración de Windows.
3.  En la pestaña usuarios y grupos, seleccione **Agregar usuario**. Debe asignar un rol de administrador de puerta de enlace o usuario de puerta de enlace para cada usuario o grupo agregado.

Una vez que Active Azure AD autenticación, el servicio de puerta de enlace se reiniciará y deberá actualizar el explorador. Puede actualizar el acceso de usuario para la aplicación de Azure AD de SME en el Azure Portal en cualquier momento.

Se solicitará a los usuarios que inicien sesión con su identidad de Azure Active Directory cuando intenten obtener acceso a la dirección URL de la puerta de enlace del centro de administración de Windows. Recuerde que los usuarios también deben ser miembros de los usuarios locales en el servidor de puerta de enlace para tener acceso al centro de administración de Windows.

Los usuarios y los administradores pueden ver su cuenta de inicio de sesión actual y así como cerrar la sesión de esta cuenta de Azure AD desde la pestaña **cuenta** de la configuración del centro de administración de Windows.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configuración de la autenticación de Azure Active Directory para el centro de administración de Windows

[Para configurar la autenticación de Azure ad, primero debe registrar la puerta de enlace con Azure](azure-integration.md) (solo tiene que hacerlo una vez para la puerta de enlace del centro de administración de Windows). En este paso se crea una aplicación Azure AD desde la que puede administrar el acceso de administrador de puerta de enlace y usuario de puerta de enlace.

Si desea conceder acceso específico a usuarios de Azure AD o administradores de puerta de enlace de usuarios o grupos al servicio del centro de administración de Windows, debe hacer lo siguiente:

1.  Vaya a la aplicación de SME Azure AD en el Azure Portal. 
    -   Al hacer clic en **Cambiar control de acceso** y seleccionar **Azure Active Directory** desde la configuración de acceso del centro de administración de Windows, puede usar el hipervínculo proporcionado en la interfaz de usuario para tener acceso a la aplicación de Azure ad en el Azure portal. Este hipervínculo también está disponible en la configuración de acceso después de hacer clic en guardar y seleccionar Azure AD como proveedor de identidades de control de acceso.
    -   También puede encontrar la aplicación en el Azure Portal si va a **Azure Active Directory** > **Enterprise Applications** > **todas las aplicaciones** y busca **SME** (la aplicación Azure ad se denominará SME-<gateway>). Si no obtiene ningún resultado de búsqueda, asegúrese de que **Mostrar** está establecido en **todas las aplicaciones**, el estado de la **aplicación** está establecido en **cualquiera** y haga clic en aplicar y, a continuación, intente realizar la búsqueda. Una vez que haya encontrado la aplicación, vaya a **usuarios y grupos**
2.  En la pestaña propiedades, establezca **asignación de usuario requerida** en sí.
    Una vez hecho esto, solo los miembros que aparecen en la pestaña **usuarios y grupos** podrán acceder a la puerta de enlace del centro de administración de Windows.
3.  En la pestaña usuarios y grupos, seleccione **Agregar usuario**. Debe asignar un rol de administrador de puerta de enlace o usuario de puerta de enlace para cada usuario o grupo agregado.

Una vez que guarde el control de acceso de Azure AD en el panel **Cambiar control de acceso** , el servicio de puerta de enlace se reinicia y debe actualizar el explorador. Puede actualizar el acceso de usuario para el centro de administración de Windows Azure AD aplicación en el Azure Portal en cualquier momento. 

Se solicitará a los usuarios que inicien sesión con su identidad de Azure Active Directory cuando intenten obtener acceso a la dirección URL de la puerta de enlace del centro de administración de Windows. Recuerde que los usuarios también deben ser miembros de los usuarios locales en el servidor de puerta de enlace para tener acceso al centro de administración de Windows. 

Mediante la pestaña **Azure** de la configuración general del centro de administración de Windows, los usuarios y los administradores pueden ver su cuenta de inicio de sesión actual y cerrar la sesión de esta cuenta de Azure ad.

### <a name="conditional-access-and-multi-factor-authentication"></a>Acceso condicional y autenticación multifactor

Una de las ventajas de usar Azure AD como una capa adicional de seguridad para controlar el acceso a la puerta de enlace del centro de administración de Windows es que puede aprovechar las eficaces características de seguridad de Azure AD, como el acceso condicional y la autenticación multifactor. 

[Obtenga más información sobre cómo configurar el acceso condicional con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurar el inicio de sesión único

**Inicio de sesión único cuando se implementa como un servicio en Windows Server**

Al instalar el centro de administración de Windows en Windows 10, está listo para usar el inicio de sesión único. Sin embargo, si va a usar el centro de administración de Windows en Windows Server, debe configurar alguna forma de delegación Kerberos en su entorno para poder usar el inicio de sesión único. La delegación configura el equipo de puerta de enlace como de confianza para delegar en el nodo de destino. 

Para configurar la [delegación restringida basada en recursos](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) en el entorno, ejecute los siguientes cmdlets de PowerShell. (Tenga en cuenta que esto requiere un controlador de dominio que ejecute Windows Server 2012 o posterior).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

En este ejemplo, la puerta de enlace del centro de administración de Windows se instala en el servidor **WindowsAdminCenterGW**y el nombre del nodo de destino es **ManagedNode**.

Para quitar esta relación, ejecute el siguiente cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Control de acceso basado en roles

El control de acceso basado en roles le permite proporcionar a los usuarios acceso limitado a la máquina en lugar de hacerlo como administradores locales completos.
[Obtenga más información sobre el control de acceso basado en roles y los roles disponibles.](../plan/user-access-options.md#role-based-access-control)

La configuración de RBAC consta de 2 pasos: habilitar la compatibilidad en los equipos de destino y asignar usuarios a los roles correspondientes.

> [!TIP]
> Asegúrese de que tiene privilegios de administrador local en las máquinas en las que está configurando la compatibilidad con el control de acceso basado en roles.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Aplicar el control de acceso basado en rol a un solo equipo

El modelo de implementación de una sola máquina es ideal para entornos sencillos con solo unos pocos equipos que administrar.
La configuración de un equipo con compatibilidad con el control de acceso basado en roles producirá los siguientes cambios:

-   Los módulos de PowerShell con las funciones requeridas por el centro de administración de Windows se instalarán en la unidad del sistema, en `C:\Program Files\WindowsPowerShell\Modules`. Todos los módulos se iniciarán con **Microsoft. SME**
-   La configuración de estado deseado ejecutará una configuración única para configurar un único punto de conexión de administración en el equipo, denominado **Microsoft. SME. PowerShell**. Este punto de conexión define los tres roles que usa el centro de administración de Windows y se ejecutará como administrador local temporal cuando un usuario se conecte a él.
-   se crearán tres nuevos grupos locales para controlar a qué usuarios se les asigna acceso a qué roles:
    -   Administradores del centro de administración de Windows
    -   Administradores de Hyper-V del centro de administración de Windows
    -   Lectores del centro de administración de Windows

Para habilitar la compatibilidad con el control de acceso basado en roles en un solo equipo, siga estos pasos:

1.  Abra el centro de administración de Windows y conéctese a la máquina que desea configurar con el control de acceso basado en roles mediante una cuenta con privilegios de administrador local en el equipo de destino.
2.  En la herramienta de **información general** , haga clic en **configuración** > **control de acceso basado en roles**.
3.  Haga clic en **aplicar** en la parte inferior de la página para habilitar la compatibilidad con el control de acceso basado en roles en el equipo de destino. El proceso de aplicación implica la copia de scripts de PowerShell y la invocación de una configuración (mediante la configuración de estado deseado de PowerShell) en el equipo de destino. Puede tardar hasta 10 minutos en completarse y se reiniciará WinRM. Esto desconectará temporalmente los usuarios del centro de administración de Windows, PowerShell y WMI.
4.  Actualice la página para comprobar el estado del control de acceso basado en rol. Cuando esté listo para su uso, el estado cambiará a **aplicado**.

Una vez aplicada la configuración, puede asignar usuarios a los roles:

1.  Abra la herramienta **usuarios y grupos locales** y navegue hasta la pestaña **grupos** .
2.  Seleccione el grupo **lectores del centro de administración de Windows** .
3.  En el panel de *detalles* de la parte inferior, haga clic en **Agregar usuario** y escriba el nombre de un usuario o grupo de seguridad que debe tener acceso de solo lectura al servidor a través del centro de administración de Windows. Los usuarios y grupos pueden provienen del equipo local o del dominio de Active Directory.
4.  Repita los pasos 2-3 para los grupos de administradores de **Hyper-V del centro** de administración de Windows y del **centro de administración de Windows** .

También puede rellenar estos grupos de forma coherente en todo el dominio configurando un objeto de directiva de grupo con la configuración de la [Directiva de grupos restringidos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Aplicar el control de acceso basado en rol a varios equipos

En una implementación empresarial de gran tamaño, puede usar las herramientas de automatización existentes para enviar la característica de control de acceso basado en roles a los equipos mediante la descarga del paquete de configuración desde la puerta de enlace del centro de administración de Windows.
El paquete de configuración está diseñado para usarse con la configuración de estado deseado de PowerShell, pero puede adaptarlo para que funcione con su solución de automatización preferida.

#### <a name="download-the-role-based-access-control-configuration"></a>Descarga de la configuración de control de acceso basado en rol

Para descargar el paquete de configuración de control de acceso basado en rol, deberá tener acceso al centro de administración de Windows y a un símbolo del sistema de PowerShell.

Si está ejecutando la puerta de enlace del centro de administración de Windows en modo de servicio en Windows Server, use el siguiente comando para descargar el paquete de configuración.
Asegúrese de actualizar la dirección de puerta de enlace con la correcta para su entorno.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Si está ejecutando la puerta de enlace del centro de administración de Windows en el equipo con Windows 10, ejecute el siguiente comando en su lugar:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Al expandir el archivo zip, verá la siguiente estructura de carpetas:

- InstallJeaFeatures. ps1
- JustEnoughAdministration (directorio)
- Módulos (directorio)
    - Microsoft. SME. \* (directorios)
    - WindowsAdminCenter. jea (directorio)

Para configurar la compatibilidad con el control de acceso basado en roles en un nodo, debe realizar las siguientes acciones:

1.  Copie los módulos JustEnoughAdministration, Microsoft. SME. \* y WindowsAdminCenter. jea en el directorio del módulo de PowerShell en el equipo de destino. Normalmente, se encuentra en `C:\Program Files\WindowsPowerShell\Modules`.
2.  Actualice el archivo **InstallJeaFeature. PS1** para que coincida con la configuración deseada para el punto de conexión de RBAC.
3.  Ejecute InstallJeaFeature. PS1 para compilar el recurso de DSC.
4.  Implemente la configuración de DSC en todas las máquinas para aplicar la configuración.

En la siguiente sección se explica cómo hacerlo mediante la comunicación remota de PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Implementación en varios equipos

Para implementar la configuración que descargó en varios equipos, deberá actualizar el script **InstallJeaFeatures. PS1** para incluir los grupos de seguridad adecuados para su entorno, copiar los archivos en cada uno de los equipos e invocar la scripts de configuración.
Puede usar sus herramientas de automatización preferidas para lograrlo; sin embargo, este artículo se centrará en un enfoque puro basado en PowerShell.

De forma predeterminada, el script de configuración creará grupos de seguridad locales en el equipo para controlar el acceso a cada uno de los roles.
Esto es adecuado para equipos de grupo de trabajo y Unidos a un dominio, pero si va a realizar la implementación en un entorno solo de dominio, es posible que quiera asociar directamente un grupo de seguridad de dominio a cada rol.
Para actualizar la configuración para usar grupos de seguridad de dominio, Abra **InstallJeaFeatures. PS1** y realice los cambios siguientes:

1.  Quite los 3 recursos de **Grupo** del archivo:
    1.  "Grupo MS-Readers-Group"
    2.  "Grupo MS-Hyper-V-Administrators-Group"
    3.  "Grupo MS-Administrators-Group"
2.  Quitar los 3 recursos de grupo de la propiedad **DEPENDSON** de JeaEndpoint
    1.  "[Grupo] MS-Readers-Group"
    2.  "[Grupo] MS-Hyper-V-Administrators-Group"
    3.  "[Grupo] MS-Administrators-Group"
3.  Cambie los nombres de grupo de la propiedad JeaEndpoint **RoleDefinitions** a los grupos de seguridad que quiera. Por ejemplo, si tiene un grupo de seguridad *CONTOSO\MyTrustedAdmins* al que se debe asignar el acceso al rol de administradores del centro de administración de Windows, cambie `'$env:COMPUTERNAME\Windows Admin Center Administrators'` a `'CONTOSO\MyTrustedAdmins'`. Las tres cadenas que necesita actualizar son:
    1.  ' $env: administradores del centro de administración de COMPUTERNAME\Windows
    2.  ' $env: COMPUTERNAME\Windows admin Center Hyper-V Administrators '
    3.  ' $env: lectores del centro de administración de COMPUTERNAME\Windows

> [!NOTE]
> Asegúrese de usar grupos de seguridad únicos para cada rol. Se producirá un error en la configuración si se asigna el mismo grupo de seguridad a varios roles.

Después, al final del archivo **InstallJeaFeatures. PS1** , agregue las siguientes líneas de PowerShell en la parte inferior del script:

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

Por último, puede copiar la carpeta que contiene los módulos, el recurso de DSC y la configuración en cada nodo de destino y ejecutar el script **InstallJeaFeature. PS1** .
Para hacerlo de forma remota desde la estación de trabajo de administración, puede ejecutar los siguientes comandos:

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
