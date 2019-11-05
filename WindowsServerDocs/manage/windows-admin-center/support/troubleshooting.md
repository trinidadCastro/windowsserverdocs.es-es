---
title: Pasos para solucionar problemas comunes de Windows Admin Center
description: Pasos para solucionar problemas comunes de Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: 0b4e02e6759bdb91ea51b5dcf5e1d0ae307d13b4
ms.sourcegitcommit: 1da993bbb7d578a542e224dde07f93adfcd2f489
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73567097"
---
# <a name="troubleshooting-windows-admin-center"></a>Solución de problemas de Windows Admin Center

> Se aplica a: Centro de administración de Windows, versión preliminar del centro de administración de Windows

> [!Important]
> Esta guía te ayudará a diagnosticar y resolver problemas que te impiden usar Windows Admin Center. Si tienes un problema con una herramienta específica, comprueba si estás experimentando un [problema conocido.](https://aka.ms/wacknownissues)

## <a name="installer-fails-with-message-_the-module-microsoftpowershelllocalaccounts-could-not-be-loaded_"></a>Error del instalador con el mensaje:  **_no se pudo cargar el módulo ' Microsoft. PowerShell. LocalAccounts '._**

Esto puede ocurrir si se ha modificado o quitado la ruta de acceso predeterminada del módulo de PowerShell. Para resolver el problema, asegúrese de que ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` sea el **primer** elemento de la variable de entorno PSModulePath. Puede lograrlo con la siguiente línea de PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>Recibo un error del tipo **No se puede acceder a este sitio/página** en el navegador web

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>Si has instalado Windows Admin Center como una **aplicación en Windows 10**

* Comprueba que se ejecuta Windows Admin Center. Busque el icono del centro de administración de Windows ![](../media/trayIcon.PNG) en la bandeja del sistema o en el **centro de administración de Windows Desktop/SmeDesktop. exe** en el administrador de tareas. De otra forma, inicia **Windows Admin Center** en el menú Inicio.

> [!NOTE] 
> Después de reiniciar, debes iniciar Windows Admin Center desde el menú Inicio.  

* [Comprobar la versión de Windows](#check-the-windows-version)

* Asegúrate de que utilizas Microsoft Edge o Google Chrome como navegador web.

* ¿Has seleccionado el certificado correcto en [¿primer inicio?](../use/get-started.md#selecting-a-client-certificate)

  * Intenta abrir el explorador en una sesión privada; si funciona, tendrás que borrar la memoria caché.

* ¿Ha actualizado recientemente Windows 10 a una nueva compilación o versión?

  * Es posible que se haya desactivado la configuración de hosts de confianza. [Siga estas instrucciones para actualizar la configuración de los hosts de confianza.](#configure-trustedhosts)

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>Si has instalado Windows Admin Center como una **puerta de enlace en Windows Server**

* [Comprueba la versión de Windows](#check-the-windows-version) del cliente y del servidor.

* Asegúrate de que utilizas Microsoft Edge o Google Chrome como navegador web.

* En el servidor, abra el administrador de tareas > servicios y asegúrese de que el **centro de administración de ServerManagementGateway/Windows** se está ejecutando.
![](../media/Service-TaskMan.PNG)

* Pruebe la conexión de red con la puerta de enlace (reemplace \<valores > con la información de la implementación)

    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>Si ha instalado el centro de administración de Windows en una máquina virtual de Azure Windows Server

* [Comprobar la versión de Windows](#check-the-windows-version)
* ¿Has agregado una regla de puerto entrante para HTTPS? 
* [Más información sobre la instalación del centro de administración de Windows en una máquina virtual de Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

### <a name="check-the-windows-version"></a>Comprobar la versión de Windows

* Abre el cuadro de diálogo Ejecutar (tecla de Windows + R) e inicia ```winver```.

* Si estás usando Windows 10 versión 1703 o anterior, Windows Admin Center no se admite en su versión de Microsoft Edge. Puedes actualizar a una versión reciente de Windows 10 o usar Chrome.

* Si usa una versión de vista previa de Insider de Windows 10 o Server con una versión de compilación entre 17134 y 17637, Windows tuvo un error que hizo que el centro de administración de Windows produjera un error. Use una versión de Windows compatible actual.

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>Asegúrese de que el servicio Administración remota de Windows (WinRM) se está ejecutando en la máquina de puerta de enlace y en el nodo administrado.

* Abrir el cuadro de diálogo de ejecución con WindowsKey + R
* Escriba ```services.msc``` y presione Entrar.
* En la ventana que se abre, busque Administración remota de Windows (WinRM), asegúrese de que se está ejecutando y de que está configurado para iniciarse automáticamente.

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>¿Actualizó el servidor de 2016 a 2019?

* Es posible que se haya desactivado la configuración de hosts de confianza. [Siga estas instrucciones para actualizar la configuración de los hosts de confianza.](#configure-trustedhosts) 

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>Obtengo el mensaje: "no se puede conectar de forma segura a esta página. Esto puede deberse a que el sitio utiliza una configuración de seguridad de TLS no actualizada o no segura.

El equipo está restringido a las conexiones HTTP/2. El centro de administración de Windows usa la autenticación integrada de Windows, que no se admite en HTTP/2. Agregue los dos valores de registro siguientes en la clave ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` en **el equipo que ejecuta el explorador** para quitar la restricción http/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>Tengo problemas con el Escritorio remoto, los eventos y las herramientas de PowerShell.

Estas tres herramientas requieren el protocolo WebSocket, que normalmente está bloqueado por servidores proxy y firewalls. Si usa Google Chrome, hay un [problema conocido](known-issues.md#google-chrome) con WebSockets y la autenticación NTLM.

## <a name="i-can-connect-to-some-servers-but-not-others"></a>Puedo conectar con algunos servidores, pero no con otros

* Inicie sesión en la máquina de puerta de enlace localmente e intente ```Enter-PSSession <machine name>``` en PowerShell, reemplazando \<nombre de equipo > por el nombre de la máquina que está intentando administrar en el centro de administración de Windows. 

* Si tu entorno utiliza un grupo de trabajo en lugar de un dominio, consulta [Estoy usando Windows Admin Center en un grupo de trabajo](#using-windows-admin-center-in-a-workgroup).

* **Con cuentas de administrador local:** si usas una cuenta de usuario local que no sea la cuenta de administrador integrada, debes habilitar la directiva en el equipo de destino ejecutando el siguiente comando en PowerShell o en un símbolo del sistema como administrador en el equipo de destino:

    ```
    REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
    ```

## <a name="using-windows-admin-center-in-a-workgroup"></a>Utilizar Windows Admin Center en un grupo de trabajo

### <a name="what-account-are-you-using"></a>¿Qué cuenta estás utilizando?
Asegúrate de que las credenciales que usas son un miembro del grupo de administradores locales del servidor de destino. En algunos casos, WinRM también requiere la pertenencia a grupos en el grupo Usuarios de Administración remota. Si estás usando una cuenta de usuario local que **no sea la cuenta de administrador integrada**, tienes que habilitar la directiva en el equipo de destino ejecutando el siguiente comando en PowerShell o en un símbolo del sistema como Administrador en el equipo de destino:

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### <a name="are-you-connecting-to-a-workgroup-machine-on-a-different-subnet"></a>¿Estás conectando a un equipo de un grupo de trabajo en una subred diferente?

Para conectar a un equipo de grupo de trabajo que no esté en la misma subred que la puerta de enlace, asegúrate de que el puerto de firewall para WinRM (TCP 5985) permita el tráfico entrante en el equipo de destino. Puedes ejecutar el siguiente comando en PowerShell o en un símbolo del sistema como administrador en el equipo de destino para crear esta regla de firewall:

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### <a name="configure-trustedhosts"></a>Configurar TrustedHosts

Al instalar Windows Admin Center, se proporciona la opción de permitir que Windows Admin Center administre la configuración de TrustedHosts de la puerta de enlace. Esto es necesario en un entorno de grupo de trabajo o cuando se usan credenciales de administrador local en un dominio. Si optas por pasar por alto esta configuración, debes configurar TrustedHosts manualmente.

**Para modificar TrustedHosts mediante comandos de PowerShell:**

1. Abre una sesión de PowerShell de administrador.
2. Mira la configuración actual de TrustedHosts:

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

   > [!WARNING]
   > Si la configuración actual de TrustedHosts no está vacía, los siguientes comandos sobrescribirán tu configuración. Se recomienda guardar la configuración actual en un archivo de texto con el siguiente comando para que puedas restaurarla en caso necesario:
   > 
   > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Establece TrustedHosts en NetBIOS, IP o el FQDN de los equipos que desees administrar:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

   > [!TIP]
   > Para una forma sencilla de establecer TrustedHosts enseguida, puedes usar un carácter comodín.
   > 
   >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. Cuando hayas terminado las pruebas, puedes emitir el comando siguiente desde una sesión de PowerShell con privilegios elevados para borrar la configuración de TrustedHosts:

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Si anteriormente habías exportado la configuración, abre el archivo, copia los valores y usa este comando:

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

Edge tiene [problemas conocidos](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) relacionados con las zonas de seguridad que afectan al inicio de sesión de Azure en el centro de administración de Windows. Si tiene problemas con las características de Azure al usar Edge, intente agregar https://login.microsoftonline.com, https://login.live.com y la dirección URL de la puerta de enlace como sitios de confianza y a sitios permitidos para la configuración del bloqueador de elementos emergentes perimetrales en el explorador del lado cliente. 

Para ello:
1. Buscar **Opciones de Internet** en el menú Inicio de Windows
2. Vaya a la pestaña **seguridad** .
3. En la opción **sitios de confianza** , haga clic en el botón **sitios** y agregue las direcciones URL en el cuadro de diálogo que se abre. Deberá agregar la dirección URL de la puerta de enlace, así como https://login.microsoftonline.com y https://login.live.com.
4. Vaya a la pestaña **privacidad** .
5. En la sección **bloqueador de elementos emergentes** , haga clic en el botón **configuración** y agregue las direcciones URL en el cuadro de diálogo que se abre. Deberá agregar la dirección URL de la puerta de enlace, así como https://login.microsoftonline.com y https://login.live.com.

## <a name="having-an-issue-with-an-azure-related-feature"></a>¿Tiene un problema con una característica relacionada con Azure?

Envíenos un correo electrónico a wacFeedbackAzure@microsoft.com con la siguiente información:
* Información general de los problemas de las preguntas que se [enumeran a continuación](#providing-feedback-on-issues).
* Describa el problema y los pasos que realizó para reproducir el problema. 
* ¿Previamente registró la puerta de enlace en Azure mediante el script descargable New-AadApp. PS1 y después ha actualizado a la versión 1807? ¿O registró la puerta de enlace en Azure con la interfaz de usuario de la configuración de puerta de enlace > Azure?
* ¿Su cuenta de Azure está asociada a varios directorios o inquilinos?
    * En caso afirmativo: al registrar la aplicación Azure AD en el centro de administración de Windows, ¿se usó el directorio predeterminado en Azure? 
* ¿Tiene su cuenta de Azure acceso a varias suscripciones?
* ¿Se ha asociado la facturación a la suscripción que estaba usando?
* ¿Ha iniciado sesión en varias cuentas de Azure en el momento en que se produjo el problema?
* ¿La cuenta de Azure requiere autenticación multifactor?
* ¿Está tratando de administrar una máquina virtual de Azure?
* ¿Está instalado el centro de administración de Windows en una máquina virtual de Azure?

## <a name="providing-feedback-on-issues"></a>Proporcionar comentarios sobre problemas

Ve a Visor de sucesos > Aplicaciones y servicios > Microsoft-ServerManagementExperience y busca errores o advertencias.

Archiva un error en nuestro [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) que describa el problema.

No olvides incluir errores o advertencias que encuentres en el registro de eventos, así como la siguiente información: 

* Plataforma donde está **instalado** Windows Admin Center (Windows 10 o Windows Server):
    * Si está instalado en el servidor, ¿cuál es la [versión](#check-the-windows-version) de Windows del **equipo que ejecuta el explorador** para tener acceso al centro de administración de Windows: 
    * ¿Usa el certificado autofirmado creado por el instalador?
    * ¿Si estás utilizando tu propio certificado, ¿coincide el nombre del firmante con el equipo?
    * Si está utilizando tu propio certificado,¿especifica un nombre de firmante alternativo?
* ¿Has instalado con la configuración de puerto predeterminada?
    * Si no es así, ¿qué puerto especificaste?
* ¿Está el equipo donde está **instalado** Windows Admin Center unido a un dominio?
* Windows [versión](#check-the-windows-version) donde está **instalado** Windows Admin Center:
* ¿Está el equipo que estás **intentando administrar** unido a un dominio?
* Windows [versión](#check-the-windows-version) de la máquina que estás **intentando administrar**:
* ¿Qué explorador utilizas?
    * Si estás utilizando Google Chrome, ¿qué versión? (Ayuda > Acerca de Google Chrome)

