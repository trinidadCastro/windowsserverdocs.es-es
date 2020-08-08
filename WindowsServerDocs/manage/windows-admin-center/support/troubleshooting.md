---
title: Pasos comunes de solución de problemas del centro de administración de Windows
description: Pasos comunes de solución de problemas del centro de administración de Windows (Project Honolulu)
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 76b171b81ff01a7a16b700d720bf289fefddf0f7
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990206"
---
# <a name="troubleshooting-windows-admin-center"></a>Solución de problemas de Windows Admin Center

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

> [!Important]
> Esta guía le ayudará a diagnosticar y resolver los problemas que impiden el uso del centro de administración de Windows. Si tiene un problema con una herramienta específica, compruebe si está experimentando un [problema conocido.](https://aka.ms/wacknownissues)

## <a name="installer-fails-with-message-_the-module-microsoftpowershelllocalaccounts-could-not-be-loaded_"></a>Error del instalador con el mensaje: ** _no se pudo cargar el módulo ' Microsoft. PowerShell. LocalAccounts '._**

Esto puede ocurrir si se ha modificado o quitado la ruta de acceso predeterminada del módulo de PowerShell. Para resolver el problema, asegúrese de que ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` es el **primer** elemento de la variable de entorno PSModulePath. Puede lograrlo con la siguiente línea de PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>Obtengo un error de **este sitio o página** en el explorador Web

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>Si ha instalado el centro de administración de Windows como una **aplicación en Windows 10**

* Asegúrese de que el centro de administración de Windows se está ejecutando. Busque el icono del centro de administración ![ de Windows icono del centro ](../media/trayIcon.PNG) de administración de Windows en la bandeja del sistema o en el **escritorio/SmeDesktop.exedel centro de administración de Windows** en el administrador de tareas. Si no es así, inicie el **centro de administración de Windows** desde el menú Inicio.

> [!NOTE]
> Después del reinicio, debe iniciar el centro de administración de Windows desde el menú Inicio.

* [Comprobar la versión de Windows](#check-the-windows-version)

* Asegúrese de que usa Microsoft Edge o Google Chrome como el explorador Web.

* ¿Ha seleccionado el certificado correcto en el [primer inicio?](../use/get-started.md#selecting-a-client-certificate)

  * Intente abrir el explorador en una sesión privada; si es así, deberá borrar la memoria caché.

* ¿Ha actualizado recientemente Windows 10 a una nueva compilación o versión?

  * Es posible que se haya desactivado la configuración de hosts de confianza. [Siga estas instrucciones para actualizar la configuración de los hosts de confianza.](#configure-trustedhosts)

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>Si ha instalado el centro de administración de Windows como **puerta de enlace en Windows Server**

* [Compruebe la versión de Windows](#check-the-windows-version) del cliente y el servidor.

* Asegúrese de que usa Microsoft Edge o Google Chrome como el explorador Web.

* En el servidor, abra el administrador de tareas > servicios y asegúrese de que el **centro de administración de ServerManagementGateway/Windows** se está ejecutando.

    ![Administrador de tareas-pestaña servicios](../media/Service-TaskMan.PNG)

* Prueba de la conexión de red a la puerta de enlace (reemplazar \<values> por la información de la implementación)

    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>Si ha instalado el centro de administración de Windows en una máquina virtual de Azure Windows Server

* [Comprobar la versión de Windows](#check-the-windows-version)
* ¿Ha agregado una regla de puerto de entrada para HTTPS?
* [Más información sobre la instalación del centro de administración de Windows en una máquina virtual de Azure](../azure/azure-integration.md)

### <a name="check-the-windows-version"></a>Comprobar la versión de Windows

* Abra el cuadro de diálogo Ejecutar (tecla Windows + R) e inicie ```winver``` .

* Si usa Windows 10 versión 1703 o inferior, no se admite el centro de administración de Windows en su versión de Microsoft Edge. Actualice a una versión reciente de Windows 10 o use Chrome.

* Si usa una versión de vista previa de Insider de Windows 10 o Server con una versión de compilación entre 17134 y 17637, Windows tuvo un error que hizo que el centro de administración de Windows produjera un error. Use una versión de Windows compatible actual.

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>Asegúrese de que el servicio Administración remota de Windows (WinRM) se está ejecutando en la máquina de puerta de enlace y en el nodo administrado.

* Abrir el cuadro de diálogo de ejecución con WindowsKey + R
* Escriba ```services.msc``` y presione Entrar.
* En la ventana que se abre, busque Administración remota de Windows (WinRM), asegúrese de que se está ejecutando y de que está configurado para iniciarse automáticamente.

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>¿Actualizó el servidor de 2016 a 2019?

* Es posible que se haya desactivado la configuración de hosts de confianza. [Siga estas instrucciones para actualizar la configuración de los hosts de confianza.](#configure-trustedhosts)

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>Obtengo el mensaje: "no se puede conectar de forma segura a esta página. Esto puede deberse a que el sitio utiliza una configuración de seguridad de TLS no actualizada o no segura.

El equipo está restringido a las conexiones HTTP/2. El centro de administración de Windows usa la autenticación integrada de Windows, que no se admite en HTTP/2. Agregue los dos valores de registro siguientes en la ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` clave del equipo en el **que se ejecuta el explorador** para quitar la restricción http/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>Tengo problemas con el Escritorio remoto, los eventos y las herramientas de PowerShell.

Estas tres herramientas requieren el protocolo WebSocket, que normalmente está bloqueado por servidores proxy y firewalls. Si usa Google Chrome, hay un [problema conocido](known-issues.md#google-chrome) con WebSockets y la autenticación NTLM.

## <a name="i-can-connect-to-some-servers-but-not-others"></a>Puedo conectarme a algunos servidores, pero no a otros

* Inicie sesión en la máquina de la puerta de enlace localmente e intente ```Enter-PSSession <machine name>``` en PowerShell, reemplazando \<machine name> por el nombre de la máquina que está intentando administrar en el centro de administración de Windows.

* Si su entorno usa un grupo de trabajo en lugar de un dominio, consulte [uso del centro de administración de Windows en un grupo de trabajo](#using-windows-admin-center-in-a-workgroup).

* **Usar cuentas de administrador local:** Si utiliza una cuenta de usuario local que no es la cuenta de administrador integrada, deberá habilitar la Directiva en el equipo de destino mediante la ejecución del siguiente comando en PowerShell o en un símbolo del sistema como administrador en el equipo de destino:

    ```
    REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
    ```

## <a name="using-windows-admin-center-in-a-workgroup"></a>Usar el centro de administración de Windows en un grupo de trabajo

### <a name="what-account-are-you-using"></a>¿Qué cuenta usa?
Asegúrese de que las credenciales que usa son miembros del grupo de administradores locales del servidor de destino. En algunos casos, WinRM también requiere la pertenencia al grupo usuarios de administración remota. Si utiliza una cuenta de usuario local que no es **la cuenta de administrador integrada**, deberá habilitar la Directiva en el equipo de destino mediante la ejecución del siguiente comando en PowerShell o en un símbolo del sistema como administrador en el equipo de destino:

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### <a name="are-you-connecting-to-a-workgroup-machine-on-a-different-subnet"></a>¿Está conectado a una máquina de grupo de trabajo en una subred diferente?

Para conectarse a un equipo de grupo de trabajo que no está en la misma subred que la puerta de enlace, asegúrese de que el puerto de Firewall para WinRM (TCP 5985) permite el tráfico de entrada en el equipo de destino. Puede ejecutar el siguiente comando en PowerShell o en un símbolo del sistema como administrador en el equipo de destino para crear esta regla de Firewall:

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### <a name="configure-trustedhosts"></a>Configurar TrustedHosts

Al instalar el centro de administración de Windows, se le ofrece la opción de permitir que el centro de administración de Windows administre la configuración de TrustedHosts de la puerta de enlace. Esto es necesario en un entorno de grupo de trabajo o cuando se usan credenciales de administrador local en un dominio. Si elige esta opción, debe configurar TrustedHosts manualmente.

**Para modificar TrustedHosts mediante comandos de PowerShell:**

1. Abra una sesión de PowerShell de administrador.
2. Vea la configuración actual de TrustedHosts:

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

   > [!WARNING]
   > Si el valor actual de TrustedHosts no está vacío, los siguientes comandos sobrescribirán la configuración. Se recomienda guardar la configuración actual en un archivo de texto con el comando siguiente para poder restaurarla si es necesario:
   >
   > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Establezca TrustedHosts en el NetBIOS, la dirección IP o el FQDN de las máquinas que desea administrar:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

   > [!TIP]
   > Para obtener una manera fácil de configurar todos los TrustedHosts a la vez, puede usar un carácter comodín.
   >
   > ```powershell
   > Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'
   > ```

4. Cuando haya terminado las pruebas, puede emitir el siguiente comando desde una sesión de PowerShell con privilegios elevados para borrar el valor de TrustedHosts:

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Si ya ha exportado la configuración, abra el archivo, copie los valores y use este comando:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>Anteriormente tenía instalado el centro de administración de Windows y ahora nada más puede usar el mismo puerto TCP/IP

Ejecute manualmente estos dos comandos en un símbolo del sistema con privilegios elevados:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

## <a name="azure-features-dont-work-properly-in-edge"></a>Las características de Azure no funcionan correctamente en Edge

Edge tiene [problemas conocidos](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) relacionados con las zonas de seguridad que afectan al inicio de sesión de Azure en el centro de administración de Windows. Si tiene problemas con las características de Azure al usar Edge, intente agregar https://login.microsoftonline.com https://login.live.com y la dirección URL de la puerta de enlace como sitios de confianza y a sitios permitidos para la configuración del bloqueador de elementos emergentes perimetrales en el explorador del lado cliente.

Para ello:
1. Buscar **Opciones de Internet** en el menú Inicio de Windows
2. Vaya a la pestaña **seguridad** .
3. En la opción **Sitios de confianza**, haga clic en el botón de **sitios** y agregue las direcciones URL en el cuadro de diálogo que se abrirá. Deberá agregar la dirección URL de la puerta de enlace, así como https://login.microsoftonline.com y https://login.live.com .
4. Vaya a la pestaña **privacidad** .
5. En la sección **bloqueador de elementos emergentes** , haga clic en el botón **configuración** y agregue las direcciones URL en el cuadro de diálogo que se abre. Deberá agregar la dirección URL de la puerta de enlace, así como https://login.microsoftonline.com y https://login.live.com .

## <a name="having-an-issue-with-an-azure-related-feature"></a>¿Tiene un problema con una característica relacionada con Azure?

Envíenos un correo electrónico a wacFeedbackAzure@microsoft.com con la siguiente información:
* Información general de los problemas de las preguntas que se [enumeran a continuación](#providing-feedback-on-issues).
* Describa el problema y los pasos que realizó para reproducir el problema.
* ¿Previamente registró la puerta de enlace en Azure mediante el New-AadApp.ps1 script descargable y, a continuación, la actualización a la versión 1807? ¿O registró la puerta de enlace en Azure con la interfaz de usuario de la configuración de puerta de enlace > Azure?
* ¿Su cuenta de Azure está asociada a varios directorios o inquilinos?
    * En caso afirmativo: al registrar la aplicación Azure AD en el centro de administración de Windows, ¿se usó el directorio predeterminado en Azure?
* ¿Tiene su cuenta de Azure acceso a varias suscripciones?
* ¿Se ha asociado la facturación a la suscripción que estaba usando?
* ¿Ha iniciado sesión en varias cuentas de Azure en el momento en que se produjo el problema?
* ¿La cuenta de Azure requiere autenticación multifactor?
* ¿Está tratando de administrar una máquina virtual de Azure?
* ¿Está instalado el centro de administración de Windows en una máquina virtual de Azure?

## <a name="providing-feedback-on-issues"></a>Proporcionar comentarios sobre problemas

Vaya a Visor de eventos > aplicaciones y servicios > Microsoft-ServerManagementExperience y busque cualquier error o advertencia.

Presente un error en el [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) que describe su problema.

Incluya los errores o advertencias que encuentre en el registro de eventos, así como la información siguiente:

* Plataforma en la que está **instalado** el centro de administración de Windows (Windows 10 o Windows Server):
    * Si está instalado en el servidor, ¿cuál es la [versión](#check-the-windows-version) de Windows del **equipo que ejecuta el explorador** para tener acceso al centro de administración de Windows:
    * ¿Usa el certificado autofirmado creado por el instalador?
    * Si usa su propio certificado, ¿el nombre del sujeto coincide con el equipo?
    * Si usa su propio certificado, ¿especifica un nombre de sujeto alternativo?
* ¿Instaló con la configuración de Puerto predeterminada?
    * Si no es así, ¿qué puerto especificó?
* ¿El equipo en el que está **instalado** el centro de administración de Windows está unido a un dominio?
* [Versión](#check-the-windows-version) de Windows en la que está **instalado**el centro de administración de Windows:
* ¿La máquina que está **intentando administrar** está unida a un dominio?
* [Versión](#check-the-windows-version) de Windows de la máquina que está **intentando administrar**:
* ¿Qué explorador utiliza?
    * Si usa Google Chrome, ¿cuál es la versión? (Ayuda > sobre Google Chrome)