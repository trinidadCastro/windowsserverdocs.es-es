---
title: Configuración de control de acceso de usuario y permisos
description: Obtén información sobre cómo configurar el control de acceso de usuario y permisos mediante Active Directory o Azure AD (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2019
ms.locfileid: "9152050"
---
# Configurar los permisos y Control de acceso de usuario

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Si no lo has hecho ya, familiarizarte con las [Opciones de control de acceso de usuario en Windows Admin Center](../plan/user-access-options.md)

>[!NOTE]
> Acceso de grupo basado en Windows Admin Center no se admite en entornos de grupo de trabajo o en varios dominios que no son de confianza.

## Definiciones de función de acceso de puerta de enlace

Hay dos funciones para acceder al servicio de puerta de enlace de Windows Admin Center:

**Los usuarios de la puerta de enlace** puede conectarse al servicio de puerta de enlace de Windows Admin Center para administrar servidores a través de dicha puerta de enlace, pero no pueden cambiar los permisos de acceso ni el mecanismo de autenticación que se usan para autenticar a la puerta de enlace.

**Los administradores de puerta de enlace** puede configurar quién obtiene acceso, así como el comportamiento de los usuarios autenticarse en la puerta de enlace. Solo los administradores de puerta de enlace pueden ver y configurar la configuración de acceso en Windows Admin Center. Los administradores locales en el equipo de puerta de enlace siempre sean administradores del servicio de puerta de enlace de Windows Admin Center.

> [!NOTE]
> Acceso a la puerta de enlace no implica el acceso a los servidores administrados visible la puerta de enlace. Para administrar un servidor de destino, el usuario que se conecta debe usar las credenciales (ya sea a través de su credencial de Windows pasan o las credenciales proporcionadas en la sesión de Windows Admin Center con la acción de **administrar como** ) que tienen acceso administrativo en ese servidor de destino.

## Active Directory o grupos de equipo local

De manera predeterminada, Active Directory o grupos de equipo local se usan para controlar el acceso de puerta de enlace. Si tienes un dominio de Active Directory, puedes administrar el usuario de la puerta de enlace y administrador acceder desde dentro de la interfaz de Windows Admin Center.

En la pestaña de **los usuarios** puede controlar quién tiene acceso a Windows Admin Center como un usuario de la puerta de enlace. De manera predeterminada, y si no se especifica un grupo de seguridad, cualquier usuario que tiene acceso a la dirección URL de la puerta de enlace tiene acceso. Una vez que agregar uno o varios grupos de seguridad a la lista de usuarios, el acceso está restringido a los miembros de esos grupos.

Si no usas un dominio de Active Directory en su entorno, acceso está controlado por el ```Users``` y ```Administrators``` grupos locales en el equipo de puerta de enlace de Windows Admin Center.

### Autenticación de tarjeta inteligente

Puede aplicar la **autenticación con tarjeta inteligente** mediante la especificación de un grupo adicionales _necesarios_ para los grupos de seguridad basada en la tarjeta inteligente. Una vez que se ha agregado un grupo de seguridad basada en la tarjeta inteligente, un usuario solo puede acceder a los servicios de Windows Admin Center si son un miembro de cualquier grupo de seguridad y un grupo de tarjeta inteligente que se incluye en la lista de usuarios.

En la pestaña **administradores** puede controlar quién tiene acceso a Windows Admin Center como un administrador de puerta de enlace. El grupo de administradores locales en el equipo siempre tendrá acceso de administrador completo y no se puede quitar de la lista. Al agregar grupos de seguridad, se definen en los miembros de esos privilegios de grupos para cambiar la configuración de la puerta de enlace de Windows Admin Center. La lista de los administradores admite la autenticación con tarjeta inteligente de la misma forma que la lista de usuarios: con la condición y para un grupo de seguridad y un grupo de tarjeta inteligente.

## Azure Active Directory

Si tu organización usa Azure Active Directory (Azure AD), puedes agregar una capa de seguridad **adicional** para Windows Admin Center al exigir autenticación de Azure AD para acceder a la puerta de enlace. Con el fin de obtener acceso a Windows Admin Center, la del usuario **cuenta de Windows** también debe tener acceso al servidor de puerta de enlace (incluso si se usa la autenticación de Azure AD). Cuando usas Azure AD, tendrás administrar los permisos de acceso de usuario y del Administrador de Windows Admin Center desde el Portal de Azure, en lugar de desde la interfaz de usuario del centro de administración de Windows.

### Acceso a Windows Admin Center cuando se habilita la autenticación de Azure AD

Dependiendo del explorador que se utiliza, algunos usuarios obtener acceso a Windows Admin Center con autenticación de Azure AD configurada recibirá un símbolo del sistema adicionales **desde el navegador** dónde deben proporcionar su Windows credenciales de cuenta de la máquina en el que Se instala Windows Admin Center. Después de escribir esa información, los usuarios recibirán el símbolo de autenticación de Azure Active Directory adicional, lo que requiere las credenciales de una cuenta de Azure que se ha concedido acceso en la aplicación de Azure AD en Azure.

> [!NOTE]
> Usuarios que cuenta de Windows tiene **derechos de administrador** en la usará de máquina de puerta de enlace no se le pida la autenticación de Azure AD.

### Configurar la autenticación de Azure Active Directory para la versión preliminar de Windows Admin Center

Ve a la **configuración**de Windows Admin Center > **acceso** y usa el modificador para alternar para activar "usar Azure Active Directory para agregar una capa de seguridad a la puerta de enlace". Si no te has registrado la puerta de enlace a Azure, se le guiará para ello en este momento.

De manera predeterminada, todos los miembros del inquilino de Azure AD tienen acceso de usuario para el servicio de puerta de enlace de Windows Admin Center. Solo los administradores locales en el equipo de puerta de enlace tienen acceso de administrador a la puerta de enlace de Windows Admin Center. Ten en cuenta que no pueden restringirse los derechos de administradores locales en el equipo de puerta de enlace, los administradores locales pueden hacer nada, independientemente de si se usa Azure AD para la autenticación.

Si quieres dar Azure AD específica a los usuarios o usuario de la puerta de enlace de grupos o acceso de administrador de puerta de enlace para el servicio de Windows Admin Center, debes hacer lo siguiente:

1.  Ve a la aplicación de Windows Admin Center Azure AD en el portal de Azure utilizando el hipervínculo proporcionado en la configuración de acceso. Ten en cuenta que este hipervínculo solo está disponible cuando está habilitada la autenticación de Azure Active Directory. 
    -   También puede encontrar la aplicación en el portal de Azure yendo a **Azure Active Directory** > **aplicaciones empresariales** > **todas las aplicaciones** y búsqueda **WindowsAdminCenter** (la aplicación de Azure AD se denominará WindowsAdminCenter-<gateway name>). Si no obtener los resultados de búsqueda, asegúrate de **Mostrar** se establece en **todas las aplicaciones**, **estado de la aplicación** se establece en **cualquier** y haga clic en aplicar, vuelva a intentar la búsqueda. Una vez que hayas encontrado la aplicación, ve a **los usuarios y grupos**
2.  En la pestaña Propiedades, Establece la **asignación de usuario necesario** en Sí.
    Una vez que hayas hecho esto, solo los miembros que aparece en la pestaña **usuarios y grupos** podrán tener acceso a la puerta de enlace de Windows Admin Center.
3.  En la ficha usuarios y grupos, selecciona **Agregar usuario**. Debes asignar un usuario de la puerta de enlace o el rol de administrador de puerta de enlace para cada usuario o grupo agregado.

Una vez que se activa la autenticación de Azure AD, se reinicia el servicio de puerta de enlace y se debe actualizar el explorador. Puedes actualizar acceso de usuario de la aplicación de SME Azure AD en el portal de Azure en cualquier momento.

Se pedirá a los usuarios inicien sesión con su identidad de Azure Active Directory al intentar acceder a la dirección URL de puerta de enlace de Windows Admin Center. Recuerda que los usuarios también deben ser un miembro de los usuarios locales en el servidor de puerta de enlace para tener acceso a Windows Admin Center.

Los usuarios y administradores pueden ver su cuenta inició sesión actualmente y, así como cerrar sesión de este Azure AD configuración de la cuenta de la pestaña de la **cuenta** de Windows Admin Center.

### Configurar la autenticación de Azure Active Directory para Windows Admin Center

[Para configurar la autenticación de Azure AD, primero debe registrar la puerta de enlace con Azure](azure-integration.md) (solo debes hacer esto una vez para la puerta de enlace de Windows Admin Center). Este paso crea una aplicación de Azure AD desde el que puedes administrar usuarios de puerta de enlace y acceso de administrador de puerta de enlace.

Si quieres dar Azure AD específica a los usuarios o usuario de la puerta de enlace de grupos o acceso de administrador de puerta de enlace para el servicio de Windows Admin Center, debes hacer lo siguiente:

1.  Ve a la aplicación de SME Azure AD en el portal de Azure. 
    -   Cuando se haga clic en el **control de acceso de cambio** y, a continuación, selecciona **Azure Active Directory** de la configuración de acceso al centro de administración de Windows, puedes usar el hipervínculo proporcionado en la interfaz de usuario para tener acceso a la aplicación de Azure AD en el portal de Azure. Este hipervínculo también está disponible en la configuración de acceso después haz clic en Guardar y has seleccionado Azure AD como el proveedor de identidad de control de acceso.
    -   También puede encontrar la aplicación en el portal de Azure yendo a **Azure Active Directory** > **aplicaciones empresariales** > **todas las aplicaciones** y búsqueda **SME** (la aplicación de Azure AD se denominará SME -<gateway>). Si no obtener los resultados de búsqueda, asegúrate de **Mostrar** se establece en **todas las aplicaciones**, **estado de la aplicación** se establece en **cualquier** y haga clic en aplicar, vuelva a intentar la búsqueda. Una vez que hayas encontrado la aplicación, ve a **los usuarios y grupos**
2.  En la pestaña Propiedades, Establece la **asignación de usuario necesario** en Sí.
    Una vez que hayas hecho esto, solo los miembros que aparece en la pestaña **usuarios y grupos** podrán tener acceso a la puerta de enlace de Windows Admin Center.
3.  En la ficha usuarios y grupos, selecciona **Agregar usuario**. Debes asignar un usuario de la puerta de enlace o el rol de administrador de puerta de enlace para cada usuario o grupo agregado.

Una vez que guarda el control de acceso de Azure AD en el panel de **control de acceso de cambio** , se reinicia el servicio de puerta de enlace y se debe actualizar el explorador. Puedes actualizar acceso de usuario de la aplicación de Windows Admin Center Azure AD en el portal de Azure en cualquier momento. 

Se pedirá a los usuarios inicien sesión con su identidad de Azure Active Directory al intentar acceder a la dirección URL de puerta de enlace de Windows Admin Center. Recuerda que los usuarios también deben ser un miembro de los usuarios locales en el servidor de puerta de enlace para tener acceso a Windows Admin Center. 

Mediante la pestaña de **Azure** de configuración general de Windows Admin Center, los usuarios y administradores pueden ver su cuenta ha iniciado la sesión actual y, así como cerrar sesión de esta cuenta de Azure AD.

### Acceso condicional y la autenticación multifactor

Una de las ventajas de usar Azure AD como una capa de seguridad adicional para controlar el acceso a la puerta de enlace de Windows Admin Center es que puede aprovechar las características de seguridad eficaz de Azure AD como acceso condicional y la autenticación multifactor. 

[Más información sobre cómo configurar el acceso condicional con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## Configurar el inicio de sesión único

**Sesión único cuando se implementa como un servicio en Windows Server**

Cuando instalas Windows Admin Center en Windows 10, está listo para usar el inicio de sesión único. Sin embargo, si vas a usar Windows Admin Center en Windows Server, tendrás que configurar alguna forma de delegación de Kerberos en el entorno antes de poder usar el inicio de sesión único. La delegación configura el equipo de puerta de enlace como de confianza para delegar en el nodo de destino. 

Para configurar [basada en recursos de la delegación restringida](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1) en su entorno, ejecute los siguientes cmdlets de PowerShell. (Estar tenga en cuenta que esto requiere un controlador de dominio que ejecute Windows Server 2012 o posterior).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

En este ejemplo, la puerta de enlace de Windows Admin Center está instalado en el servidor **WindowsAdminCenterGW**y el nombre de nodo de destino es **ManagedNode**.

Para quitar esta relación, ejecuta el siguiente cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## Control de acceso basado en roles

Control de acceso basado en roles permite proporcionar a los usuarios con acceso limitado a la máquina en lugar de hacer que los administradores locales completa.
[Más información acerca de los roles disponibles y el control de acceso basado en roles.](../plan/user-access-options.md#role-based-access-control)

Configuración de RBAC consta de los 2 pasos: habilitar la compatibilidad en los equipos de destino y asignar usuarios a las funciones relevantes.

> [!TIP]
> Asegúrate de que tienes privilegios de administrador local en los equipos que estás configurando la compatibilidad para control de acceso basado en roles.

### Control de acceso basado en roles se aplican a un solo equipo

El modelo de implementación de la misma máquina es ideal para entornos simples con solo unos pocos equipos para administrar.
Configuración de una máquina con compatibilidad para control de acceso basado en roles, se producirá en los siguientes cambios:
-   Los módulos de PowerShell con funciones requeridos por Windows Admin Center, se instalará en la unidad del sistema, en `C:\Program Files\WindowsPowerShell\Modules`. Todos los módulos se iniciarán con **Microsoft.Sme**
-   Configuración de estado deseado se ejecutará una configuración única para configurar un punto de conexión de Just Enough Administration en la máquina, denominada **Microsoft.Sme.PowerShell**. Este punto de conexión define las funciones de 3 usadas por Windows Admin Center y se ejecutará como administrador local temporal cuando un usuario se conecta a ella.
-   3 nuevos grupos locales se creará para controlar qué usuarios se les asignan acceso a qué funciones:
    -   Administradores de TI de Windows Admin Center
    -   Administradores de Hyper-V de Windows Admin Center
    -   Lectores de Windows Admin Center

Para habilitar la compatibilidad con control de acceso basado en roles en un solo equipo, sigue estos pasos:

1.  Abre Windows Admin Center y conectar con el equipo que deseas configurar con el control de acceso basado en roles con una cuenta con privilegios de administrador local en el equipo de destino.
2.  En la herramienta de **información general** , haz clic en **configuración** > **control de acceso basado en roles**.
3.  Haz clic en **Aplicar** en la parte inferior de la página para habilitar la compatibilidad con control de acceso basado en roles en el equipo de destino. El proceso de aplicación implica copiar scripts de PowerShell e invocar una configuración (con la configuración de estado deseado de PowerShell) en el equipo de destino. Puede tardar hasta 10 minutos en completarse y dará como resultado WinRM reiniciar. Esto desconectará temporalmente a los usuarios de Windows Admin Center, PowerShell y WMI.
4.  Actualizar la página para comprobar el estado del control de acceso basado en roles. Cuando esté listo para su uso, el estado cambiará a **aplicado**.

Una vez que se aplique la configuración, puedes asignar a usuarios a las funciones:

1.  Abrir la herramienta de **usuarios y grupos locales** y navega a la ficha **grupos** .
2.  Selecciona el grupo de **Windows Admin Center lectores** .
3.  En el panel de *Detalles* en la parte inferior, haz clic en **Agregar usuario** y escribe el nombre de un usuario o grupo de seguridad que debe tener acceso de solo lectura al servidor a través de Windows Admin Center. Los usuarios y grupos pueden proceder de la máquina local o el dominio de Active Directory.
4.  Repite los pasos 2 y 3 para los grupos de **Administradores de Hyper-V de Windows Admin Center** y **Los administradores de Windows Admin Center** .

También puede rellenar estos grupos de forma coherente en todo el dominio mediante la configuración de un objeto de directiva de grupo con la [Configuración de directiva de grupos restringidos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### Aplicar el control de acceso basado en roles en varios equipos

En una implementación de grandes empresas, puedes usar las herramientas existentes de automatización para insertar un vistazo a la función de control de acceso basado en roles en los equipos, descarga el paquete de configuración de la puerta de enlace de Windows Admin Center.
El paquete de configuración está diseñado para usarse con la configuración de estado deseado de PowerShell, pero puedes adaptar que funcione con la solución de automatización preferido.

#### Descargar la configuración de control de acceso basado en roles

Para descargar el paquete de configuración de control de acceso basado en roles, tendrás que tener acceso a Windows Admin Center y un símbolo del sistema de PowerShell.

Si estás ejecutando la puerta de enlace de Windows Admin Center en modo de servicio en Windows Server, usa el siguiente comando para descargar el paquete de configuración.
Asegúrate de actualizar la dirección de puerta de enlace con el correcto para tu entorno.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Si estás ejecutando la puerta de enlace de Windows Admin Center en la máquina de Windows 10, ejecute el siguiente comando en su lugar:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Cuando se expande el archivo zip, verás la estructura de carpetas siguiente:
- InstallJeaFeatures.ps1
- JustEnoughAdministration (directorio)
- Módulos (directorio)
    - Microsoft.SME.\* (directorios)
    - WindowsAdminCenter.Jea (directorio)

Para configurar la compatibilidad para control de acceso basado en roles en un nodo, debes realizar las siguientes acciones:
1.  Copia los módulos JustEnoughAdministration, Microsoft.SME.\* y WindowsAdminCenter.Jea en el directorio de módulo de PowerShell en el equipo de destino. Por lo general, esto se encuentra en `C:\Program Files\WindowsPowerShell\Modules`.
2.  Actualizar el archivo de **InstallJeaFeature.ps1** para que coincidan con la configuración deseada para el punto de conexión RBAC.
3.  Ejecutar InstallJeaFeature.ps1 para compilar el recurso de DSC.
4.  Implementar la configuración de DSC en todos los equipos para aplicar la configuración.

En la siguiente sección se explica cómo hacerlo mediante la comunicación remota de PowerShell.

#### Implementar en varios equipos

Para implementar la configuración que has descargado en varios equipos, tendrás que actualizar el script **InstallJeaFeatures.ps1** para incluir los grupos de seguridad adecuados para tu entorno, copia los archivos en cada uno de los equipos e invocar el scripts de configuración.
Puedes usar el conjunto de herramientas de automatización preferido para lograr esto, sin embargo, en este artículo se centrará en un enfoque basado en PowerShell puro.

De manera predeterminada, el script de configuración creará los grupos de seguridad local en el equipo para controlar el acceso a cada uno de los roles.
Esto es apropiado para el grupo de trabajo y dominio unido a máquinas, pero si estás realizando la implementación en un entorno de dominio de solo es posible que desees directamente asociar un grupo de seguridad de dominio a cada rol.
Para actualizar la configuración para usar grupos de seguridad de dominio, abre **InstallJeaFeatures.ps1** y realiza los siguientes cambios:

1.  Quitar los recursos del **grupo** 3 del archivo:
    1.  "Grupo de lectores de grupo MS"
    2.  "Grupo MS-Hyper-V--grupo Administradores"
    3.  "Grupo de administradores de grupo MS"
2.  Quitar los recursos del grupo 3 de la propiedad JeaEndpoint **DependsOn**
    1.  "["Grupo] MS-lectores-Group
    2.  "["Grupo] MS-Hyper-V--grupo de administradores
    3.  "["Grupo] MS grupo de administradores
3.  Cambiar los nombres de grupo en la propiedad JeaEndpoint **RoleDefinitions** a los grupos de seguridad deseado. Por ejemplo, si tienes un grupo de seguridad *CONTOSO\MyTrustedAdmins* que deben asignarse de acceso a la función de los administradores de TI de Windows Admin Center, cambiar `'$env:COMPUTERNAME\Windows Admin Center Administrators'` a `'CONTOSO\MyTrustedAdmins'`. Las tres cadenas que tengas que actualizar son:
    1.  ' $env: los administradores del centro de administración COMPUTERNAME\Windows
    2.  ' $env: administradores de Hyper-V de COMPUTERNAME\Windows Admin Center
    3.  ' $env: lectores de COMPUTERNAME\Windows Admin Center

> [!NOTE]
> Asegúrate de usar grupos de seguridad únicos para cada rol. Se producirá un error en la configuración si el mismo grupo de seguridad está asignado a varias funciones.

A continuación, al final del archivo **InstallJeaFeatures.ps1** , agrega las siguientes líneas de PowerShell en la parte inferior de la secuencia de comandos:

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

Por último, puedes copiar la carpeta que contiene los módulos, recursos de DSC y configuraciones a cada nodo de destino y ejecutar el script **InstallJeaFeature.ps1** .
Para ello de forma remota la estación de trabajo de administración, puedes ejecutar los siguientes comandos:

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
