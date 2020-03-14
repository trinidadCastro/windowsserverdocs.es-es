---
title: Configuración de permisos y controles de acceso de usuarios
description: Aprende a configurar los permisos y el control de acceso de usuarios mediante Active Directory o Azure AD (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 39af45506ff7023cebe437992e90f6d4ec051333
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323597"
---
# <a name="configure-user-access-control-and-permissions"></a>Configuración de los permisos y el control de acceso de usuarios

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Si aún no lo has hecho, familiarízate con las [opciones de control de acceso de usuarios de Windows Admin Center](../plan/user-access-options.md)

> [!NOTE]
> El acceso basado en grupos de Windows Admin Center no se admite en entornos de grupo de trabajo ni en dominios que no son de confianza.

## <a name="gateway-access-role-definitions"></a>Definiciones de roles de acceso de puerta de enlace

Hay dos roles para tener acceso al servicio de puerta de enlace de Windows Admin Center:

Los **usuarios de puerta de enlace** pueden conectarse al servicio de puerta de enlace de Windows Admin Center para administrar servidores mediante la puerta de enlace, pero no pueden cambiar los permisos de acceso ni el mecanismo de autenticación que se usa para autenticarse en la puerta de enlace.

Los **administradores de puerta de enlace** pueden configurar quién obtiene el acceso y cómo se autentican los usuarios en la puerta de enlace. Solo los administradores de puerta de enlace pueden ver y configurar la configuración de acceso en Windows Admin Center. Los administradores locales de la máquina de puerta de enlace siempre son administradores del servicio de puerta de enlace de Windows Admin Center.

> [!NOTE]
> El acceso a la puerta de enlace no implica el acceso a los servidores administrados visibles para la puerta de enlace. Para administrar un servidor de destino, el usuario que se conecta debe usar credenciales (ya sean las credenciales de paso a través de Windows o las credenciales proporcionadas en la sesión de Windows Admin Center mediante la acción **Administrar como**) con acceso administrativo al servidor de destino.

## <a name="active-directory-or-local-machine-groups"></a>Active Directory o grupos de máquinas locales

De forma predeterminada, Active Directory o los grupos de máquinas locales se usan para controlar el acceso a la puerta de enlace. Si tienes un dominio de Active Directory, puedes administrar el acceso de usuario y administrador de la puerta de enlace desde la interfaz de Windows Admin Center.

Desde la pestaña **Usuarios**, puedes controlar quién puede tener acceso a Windows Admin Center como usuario de la puerta de enlace. De forma predeterminada, y si no especificas ningún grupo de seguridad, cualquier usuario que tenga acceso a la dirección URL de la puerta de enlace tiene acceso. Una vez que se agregan uno o varios grupos de seguridad a la lista de usuarios, el acceso está restringido a los miembros de esos grupos.

Si no usas un dominio de Active Directory en el entorno, el acceso se controla mediante los grupos locales `Users` y `Administrators` de la máquina de la puerta de enlace de Windows Admin Center.

### <a name="smartcard-authentication"></a>Autenticación de tarjeta inteligente

Para aplicar la **autenticación de tarjeta inteligente**, especifica un grupo adicional _requerido_ para los grupos de seguridad basados en tarjeta inteligente. Una vez agregado un grupo de seguridad basado en tarjeta inteligente, un usuario solo puede tener acceso al servicio de Windows Admin Center si es miembro de cualquier grupo de seguridad Y un grupo de tarjetas inteligentes incluido en la lista de usuarios.

Desde la pestaña **Administradores**, puedes controlar quién puede tener acceso a Windows Admin Center como administrador de la puerta de enlace. El grupo de administradores locales en el equipo siempre tendrá acceso de administrador completo y no se puede quitar de la lista. Al agregar grupos de seguridad, concedes a los miembros de esos grupos privilegios para cambiar la configuración de la puerta de enlace de Windows Admin Center. La lista de administradores admite la autenticación mediante tarjeta inteligente de la misma forma que la lista de usuarios: con la condición Y para un grupo de seguridad y un grupo de tarjetas inteligentes.

## <a name="azure-active-directory"></a>Azure Active Directory

Si tu organización usa Azure Active Directory (Azure AD), puedes elegir agregar un nivel **adicional** de seguridad a Windows Admin Center solicitando la autenticación de Azure AD para acceder a la puerta de enlace. Para obtener acceso a Windows Admin Center, la **cuenta de Windows** del usuario también debe tener acceso al servidor de puerta de enlace (aunque se use la autenticación de Azure AD). Al usar Azure AD, podrás administrar los permisos de acceso de usuario y administrador de Windows Admin Center desde Azure Portal, en lugar de hacerlo desde la interfaz de usuario de Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Acceso a Windows Admin Center cuando está habilitada la autenticación de Azure AD

En función del explorador que uses, algunos usuarios que accedan a Windows Admin Center con la autenticación de Azure AD configurada recibirán un mensaje adicional **del explorador** para que proporcionen sus credenciales de la cuenta de Windows para la máquina en que está instalado Windows Admin Center. Después de escribir esa información, los usuarios obtendrán la solicitud de autenticación adicional de Azure Active Directory, que requiere las credenciales de una cuenta de Azure a la que se ha concedido acceso en la aplicación de Azure AD en Azure.

> [!NOTE]
> No se solicitará la autenticación de Azure AD a los usuarios cuya cuenta de Windows tiene **derechos de administrador** en la máquina de la puerta de enlace.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configuración de la autenticación de Azure Active Directory para la versión preliminar de Windows Admin Center

Ve a Windows Admin Center **Configuración** > **Acceso** y usa el modificador de alternancia para activar "Use Azure Active Directory to add a layer of security to the gateway" (Usar Azure Active Directory para agregar un nivel de seguridad a la puerta de enlace). Si no has registrado la puerta de enlace en Azure, se te guiará para que lo hagas en este momento.

De forma predeterminada, todos los miembros del inquilino de Azure AD tienen acceso de usuario al servicio de puerta de enlace de Windows Admin Center. Solo los administradores locales de la máquina de puerta de enlace tienen acceso de administrador a la puerta de enlace de Windows Admin Center. Ten en cuenta que los derechos de los administradores locales en la máquina de la puerta de enlace no se pueden restringir: los administradores locales pueden hacer cualquier cosa, independientemente de si se usa Azure AD para la autenticación.

Si quieres conceder a usuarios o grupos específicos de Azure AD acceso de administrador o usuario de puerta de enlace al servicio Windows Admin Center, antes debes seguir estos pasos:

1.  Ve a la aplicación de Azure AD de Windows Admin Center desde el hipervínculo proporcionado en la configuración de acceso. Ten en cuenta que este hipervínculo solo está disponible cuando está habilitada la autenticación de Azure Active Directory. 
    -   También puedes buscar la aplicación en Azure Portal. Para ello, ve a **Azure Active Directory** > **Aplicaciones empresariales** > **Todas las aplicaciones** y busca **WindowsAdminCenter** (la aplicación de Azure AD se denominará WindowsAdminCenter-<gateway name>). Si no obtienes ningún resultado de búsqueda, asegúrate de que la opción **Mostrar** está establecida en **todas las aplicaciones**, **estado de la aplicación** en **cualquiera** y haz clic en Aplicar. A continuación, intenta realizar la búsqueda. Una vez que hayas encontrado la aplicación, ve a **Usuarios y grupos**
2.  En la pestaña Propiedades, establece **Asignación de usuarios necesaria** en Sí.
    Una vez hecho esto, solo los miembros que se muestran en la pestaña **Usuarios y grupos** podrán acceder a la puerta de enlace de Windows Admin Center.
3.  En la pestaña Usuarios y grupos, selecciona **Agregar usuario**. Debes asignar un rol de administrador o usuario de puerta de enlace a cada usuario o grupo agregado.

Una vez activada la autenticación de Azure AD, se reiniciará el servicio de la puerta de enlace y deberás actualizar el explorador. Puedes actualizar el acceso de usuario para la aplicación de Azure AD de SME en Azure Portal en cualquier momento.

Se solicitará a los usuarios que inicien sesión con su identidad de Azure Active Directory cuando intenten acceder a la dirección URL de la puerta de enlace de Windows Admin Center. Recuerda que los usuarios también deben ser miembros del grupo de usuarios locales del servidor de la puerta de enlace para acceder a Windows Admin Center.

Los usuarios y los administradores pueden ver su cuenta con la sesión iniciada y cerrar la sesión de esta cuenta de Azure AD desde la pestaña **Cuenta** de la configuración de Windows Admin Center.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configuración de la autenticación de Azure Active Directory para Windows Admin Center

[Para configurar la autenticación de Azure AD, antes debes registrar la puerta de enlace con Azure](azure-integration.md) (solo tienes que hacerlo una vez para la puerta de enlace de Windows Admin Center). En este paso, se crea una aplicación de Azure AD desde la que puedes administrar el acceso de administrador y usuario de la puerta de enlace.

Si quieres conceder a usuarios o grupos específicos de Azure AD acceso de administrador o usuario de puerta de enlace al servicio Windows Admin Center, antes debes seguir estos pasos:

1.  Ve a la aplicación de Azure AD de SME en Azure Portal. 
    -   Al hacer clic en **Cambiar control de acceso** y seleccionar **Azure Active Directory** en la configuración de acceso de Windows Admin Center, puedes usar el hipervínculo proporcionado en la interfaz de usuario para acceder a la aplicación de Azure AD en Azure Portal. Este hipervínculo también está disponible en la configuración del Acceso después de hacer clic en Guardar y seleccionar Azure AD como proveedor de identidades de control de acceso.
    -   También puedes buscar la aplicación en Azure Portal. Para ello, ve a **Azure Active Directory** > **Aplicaciones empresariales** > **Todas las aplicaciones** y busca **SME** (la aplicación de Azure AD se denominará SME-<gateway>). Si no obtienes ningún resultado de búsqueda, asegúrate de que la opción **Mostrar** está establecida en **todas las aplicaciones**, **estado de la aplicación** en **cualquiera** y haz clic en Aplicar. A continuación, intenta realizar la búsqueda. Una vez que hayas encontrado la aplicación, ve a **Usuarios y grupos**
2.  En la pestaña Propiedades, establece **Asignación de usuarios necesaria** en Sí.
    Una vez hecho esto, solo los miembros que se muestran en la pestaña **Usuarios y grupos** podrán acceder a la puerta de enlace de Windows Admin Center.
3.  En la pestaña Usuarios y grupos, selecciona **Agregar usuario**. Debes asignar un rol de administrador o usuario de puerta de enlace para cada usuario o grupo agregado.

Una vez guardado el control de acceso de Azure AD en el panel **Cambiar control de acceso**, el servicio de puerta de enlace se reinicia y debes actualizar el explorador. Puedes actualizar el acceso de usuario para la aplicación de Azure AD de Windows Admin Center en Azure Portal en cualquier momento. 

Se solicitará a los usuarios que inicien sesión con su identidad de Azure Active Directory cuando intenten acceder a la dirección URL de la puerta de enlace de Windows Admin Center. Recuerda que los usuarios también deben ser miembros del grupo de usuarios locales del servidor de la puerta de enlace para acceder a Windows Admin Center. 

Desde la pestaña **Azure** de la configuración general de Windows Admin Center, los usuarios y los administradores pueden ver su cuenta con la sesión iniciada y cerrar la sesión de esta cuenta de Azure AD.

### <a name="conditional-access-and-multi-factor-authentication"></a>Acceso condicional y autenticación multifactor

Una de las ventajas de usar Azure AD como un nivel adicional de seguridad para controlar el acceso a la puerta de enlace de Windows Admin Center es que puedes aprovechar las eficaces características de seguridad de Azure AD, como el acceso condicional y la autenticación multifactor. 

[Más información sobre cómo configurar el acceso condicional con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurar el inicio de sesión único

**Inicio de sesión único cuando se implementa como servicio en Windows Server**

Al instalar Windows Admin Center en Windows 10, ya está listo para usar el inicio de sesión único. Sin embargo, si vas a usar Windows Admin Center en Windows Server, debes configurar alguna forma de delegación de Kerberos en tu entorno para poder usar el inicio de sesión único. La delegación configura el equipo de puerta de enlace como de confianza para delegar en el nodo de destino. 

Para configurar la [delegación restringida basada en recursos](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) en el entorno, usa el siguiente ejemplo de PowerShell. En este ejemplo se muestra cómo configurar Windows Server [node01.contoso.com] para aceptar la delegación de la puerta de enlace de Windows Admin Center [wac.contoso.com] en el dominio contoso.com.

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount (Get-ADComputer wac)
```

Para quitar esta relación, ejecuta el siguiente cmdlet:

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Control de acceso basado en roles

El control de acceso basado en rol te permite proporcionar a los usuarios acceso limitado a la máquina, en lugar de hacerles administradores locales completos.
[Más información sobre el control de acceso basado en rol y los roles disponibles.](../plan/user-access-options.md#role-based-access-control)

La configuración de RBAC consta de 2 pasos: habilitar la compatibilidad en los equipos de destino y asignar usuarios a los roles correspondientes.

> [!TIP]
> Asegúrate de tener privilegios de administrador local en las máquinas en las que estás configurando la compatibilidad con el control de acceso basado en rol.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Aplicar el control de acceso basado en rol a una sola máquina

El modelo de implementación de una sola máquina es ideal para entornos sencillos con unos pocos equipos que administrar.
La configuración de una máquina con compatibilidad con el control de acceso basado en rol provocará los siguientes cambios:

-   Los módulos de PowerShell con las funciones que requiere Windows Admin Center se instalarán en la unidad del sistema, en `C:\Program Files\WindowsPowerShell\Modules`. Todos los módulos se iniciarán con **Microsoft.Sme**.
-   La configuración de estado deseado ejecutará una configuración única para configurar un punto de conexión Just Enough Administration en la máquina denominado **Microsoft.Sme.PowerShell**. Este punto de conexión define los tres roles que usa Windows Admin Center y se ejecutará como administrador local temporal cuando un usuario se conecte a él.
-   Se crearán tres nuevos grupos locales para controlar a qué usuarios se les asigna acceso a qué roles:
    -   Administradores de Windows Admin Center
    -   Administradores de Hyper-V de Windows Admin Center
    -   Lectores de Windows Admin Center

Para habilitar la compatibilidad con el control de acceso basado en roles en una única máquina, sigue estos pasos:

1.  Abre Windows Admin Center y conéctate a la máquina que quieres configurar con el control de acceso basado en rol mediante una cuenta con privilegios de administrador local en el equipo de destino.
2.  En la herramienta **Información general**, haz clic en **Configuración** > **Control de acceso basado en rol**.
3.  Haz clic en **Aplicar** en la parte inferior de la página para habilitar la compatibilidad con el control de acceso basado en rol en el equipo de destino. El proceso de aplicación implica copiar los scripts de PowerShell e invocar una configuración (mediante la configuración de estado deseado de PowerShell) en el equipo de destino. Puede tardar hasta 10 minutos en completarse y se reiniciará WinRM. Esto desconectará temporalmente a los usuarios de Windows Admin Center, PowerShell y WMI.
4.  Actualiza la página para comprobar el estado del control de acceso basado en rol. Cuando esté listo para su uso, el estado cambiará a **Aplicada**.

Una vez aplicada la configuración, puedes asignar usuarios a los roles:

1.  Abre la herramienta **Usuarios y grupos locales** y desplázate hasta la pestaña **Grupos**.
2.  Selecciona el grupo **Lectores de Windows Admin Center**.
3.  En el panel *Detalles* de la parte inferior, haz clic en **Agregar usuario** y escribe el nombre de un usuario o grupo de seguridad que debe tener acceso de solo lectura al servidor mediante Windows Admin Center. Los usuarios y grupos pueden proceder de la máquina local o del dominio de Active Directory.
4.  Repite los pasos 2-3 para los grupos de **Administradores de Hyper-V de Windows Admin Center** y **Administradores de Windows Admin Center**.

También puedes rellenar estos grupos de forma coherente en todo el dominio configurando un objeto de directiva de grupo con la [Configuración de directiva de grupos restringidos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Aplicar el control de acceso basado en rol a varias máquinas

En una implementación empresarial de gran tamaño, puedes usar las herramientas de automatización existentes para insertar la característica de control de acceso basado en rol en los equipos mediante la descarga del paquete de configuración desde la puerta de enlace de Windows Admin Center.
El paquete de configuración está diseñado para usarse con la configuración de estado deseado de PowerShell, pero puedes adaptarlo para que funcione con tu solución de automatización preferida.

#### <a name="download-the-role-based-access-control-configuration"></a>Descargar la configuración de control de acceso basado en rol

Para descargar el paquete de configuración de control de acceso basado en rol, debes tener acceso a Windows Admin Center y un símbolo del sistema de PowerShell.

Si estás ejecutando la puerta de enlace de Windows Admin Center en modo de servicio en Windows Server, usa el siguiente comando para descargar el paquete de configuración.
Asegúrate de actualizar la dirección de puerta de enlace con la correcta para tu entorno.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Si estás ejecutando la puerta de enlace de Windows Admin Center en la máquina con Windows 10, ejecuta el siguiente comando:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Al expandir el archivo ZIP, verás la siguiente estructura de carpetas:

- InstallJeaFeatures.ps1
- JustEnoughAdministration (directorio)
- Modules (directorio)
    - Microsoft.SME.\* (directorios)
    - WindowsAdminCenter.Jea (directorio)

Para configurar la compatibilidad con el control de acceso basado en rol en un nodo, debes completar las siguientes acciones:

1.  Copia los módulos JustEnoughAdministration, Microsoft.SME.\* y WindowsAdminCenter.Jea en el directorio del módulo de PowerShell de la máquina de destino. Normalmente, se encuentra en `C:\Program Files\WindowsPowerShell\Modules`.
2.  Actualiza el archivo **InstallJeaFeature.ps1** para que coincida con la configuración deseada para el punto de conexión de RBAC.
3.  Ejecuta InstallJeaFeature.ps1 para compilar el recurso de DSC.
4.  Implementa la configuración de DSC en todas las máquinas para aplicar la configuración.

En la siguiente sección se explica cómo hacerlo mediante la comunicación remota de PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Implementación en varias máquinas

Para implementar la configuración descargada en varias máquinas, tendrás que actualizar el script **InstallJeaFeatures.ps1** para incluir los grupos de seguridad adecuados para tu entorno, copiar los archivos en cada uno de los equipos e invocar los scripts de configuración.
Puedes usar tus herramientas de automatización preferidas para lograrlo; sin embargo, este artículo se centrará en un enfoque puro basado en PowerShell.

De forma predeterminada, el script de configuración creará grupos de seguridad locales en la máquina para controlar el acceso a cada uno de los roles.
Esto es adecuado para máquinas unidas a dominios y grupos de trabajo, pero si vas a realizar la implementación en un entorno solo de dominio, es recomendable asociar directamente un grupo de seguridad de dominio con cada rol.
Para actualizar la configuración para usar grupos de seguridad de dominio, abre **InstallJeaFeatures.ps1** y aplica los cambios siguientes:

1.  Quita los 3 recursos de **grupo** del archivo:
    1.  "Group MS-Readers-Group"
    2.  "Group MS-Hyper-V-Administrators-Group"
    3.  "Group MS-Administrators-Group"
2.  Quitar los 3 recursos de grupo de la propiedad de JeaEndpoint **DependsOn**
    1.  "[Group]MS-Readers-Group"
    2.  "[Group]MS-Hyper-V-Administrators-Group"
    3.  "[Group]MS-Administrators-Group"
3.  Cambia los nombres de grupo de la propiedad de JeaEndpoint **RoleDefinitions** a los grupos de seguridad que quieras. Por ejemplo, si tienes un grupo de seguridad *CONTOSO\MyTrustedAdmins* al que se debe asignar acceso al rol Administradores de Windows Admin Center, cambia `'$env:COMPUTERNAME\Windows Admin Center Administrators'` a `'CONTOSO\MyTrustedAdmins'`. Las tres cadenas que necesitas actualizar son:
    1.  "$env:COMPUTERNAME\Windows Admin Center Administrators"
    2.  "$env:COMPUTERNAME\Windows Admin Center Administrators"
    3.  "$env:COMPUTERNAME\Windows Admin Center Administrators"

> [!NOTE]
> Asegúrate de utilizar grupos de seguridad únicos para cada rol. Se producirá un error en la configuración si se asigna el mismo grupo de seguridad a varios roles.

A continuación, al final del archivo **InstallJeaFeatures.ps1**, agrega las siguientes líneas de PowerShell a la parte inferior del script:

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

Por último, puedes copiar la carpeta que contiene los módulos, el recurso de DSC y la configuración en cada nodo de destino y ejecutar el script **InstallJeaFeature.ps1**.
Para hacerlo de forma remota desde la estación de trabajo de administración, puedes ejecutar los siguientes comandos:

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
