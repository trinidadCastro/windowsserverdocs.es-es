---
title: Pasos para solucionar problemas comunes de Windows Admin Center
description: Pasos para solucionar problemas comunes de Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/12/2019
ms.openlocfilehash: 53c943ee3eddbe8f67bec125961eb3d36ead17a3
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034471"
---
# <a name="troubleshooting-windows-admin-center"></a>Solución de problemas de Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

> [!Important]
> Esta guía te ayudará a diagnosticar y resolver problemas que te impiden usar Windows Admin Center. Si tienes un problema con una herramienta específica, comprueba si estás experimentando un [problema conocido.](http://aka.ms/wacknownissues)

<a id="toc"></a>

## <a name="quick-links"></a>Vínculos rápidos

* [Se produce un error en el programa de instalación con el mensaje: **_No se pudo cargar el módulo 'Microsoft.PowerShell.LocalAccounts'._** ](#psmodulepath)

* Recibo un error del tipo **No se puede acceder a este sitio/página** en el navegador web (selecciona el tipo de implementación)
    * [Tengo Windows Admin Center instalado como una aplicación en Windows 10](#whitescreenw10)
    * [Tengo Windows Admin Center instalado como una puerta de enlace en Windows Server](#whitescreenws)
    * [Tengo Windows Admin Center instalado como una puerta de enlace en una máquina virtual de Azure](#if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm)

* [Cargas de página principal de Windows Admin Center, pero me estoy atascado en el panel Agregar conexión o no se puede conectar a cualquier máquina.](#winvercompat)

* [Puede obtener el mensaje: "Error al cargar el módulo. Rpc: Caducadas reintentos haga "Ping"."](#winvercompat)

* [Puede obtener el mensaje: "No se puede conectar de forma segura a esta página. Puede que el sitio utiliza la configuración de seguridad TLS obsoleta o no seguro."](#tls)

* [Tengo problemas con las herramientas de escritorio remoto, eventos y PowerShell.](#websockets)

* [Tengo problemas con las características de Azure de Edge](#azlogin)

* [¿Puedo conectarme a algunos servidores, pero no otros](#connectionissues)

* [Estoy usando Windows Admin Center en un **grupo de trabajo**](#workgroup)

* [Windows Admin Center instalado previamente, y ahora nada puede usar el mismo puerto TCP/IP](#urlacl)

* [Mi problema no aparece aquí, o los pasos de esta página no resuelven el problema.](#filebug)

<a id="psmodulepath"></a>

## <a name="installer-fails-with-message-the-module-microsoftpowershelllocalaccounts-could-not-be-loaded"></a>Se produce un error en el instalador con mensaje: **_No se pudo cargar el módulo 'Microsoft.PowerShell.LocalAccounts'._** 

Esto puede ocurrir si se ha modificado o quitado su ruta de acceso del módulo de PowerShell de forma predeterminada. Para resolver el problema, asegúrese de que ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` es el **primera** elemento en la variable de entorno PSModulePath. Puede lograr esto con la siguiente línea de PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>Recibo un error del tipo **No se puede acceder a este sitio/página** en el navegador web

<a id="whitescreenw10"></a>

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>Si has instalado Windows Admin Center como una **aplicación en Windows 10**

* Comprueba que se ejecuta Windows Admin Center. Busque el icono de Windows Admin Center ![](../media/trayIcon.PNG) en la bandeja del sistema o **Windows Admin Center escritorio / SmeDesktop.exe** en el Administrador de tareas. De otra forma, inicia **Windows Admin Center** en el menú Inicio.

> [!NOTE] 
> Después de reiniciar, debes iniciar Windows Admin Center desde el menú Inicio.  

* [Comprobar la versión de Windows](#winvercompat)

* Asegúrate de que utilizas Microsoft Edge o Google Chrome como navegador web.

* ¿Has seleccionado el certificado correcto en [¿primer inicio?](../use/get-started.md#selecting-a-client-certificate)

  * Intenta abrir el explorador en una sesión privada; si funciona, tendrás que borrar la memoria caché.

* ¿Actualizó recientemente de Windows 10 a una nueva compilación o versión?

  * Esto es posible que haya desactivado la configuración de hosts de confianza. [Siga estas instrucciones para actualizar la configuración de hosts de confianza.](#configure-trustedhosts) 

[[volver al principio]](#toc)

<a id="whitescreenws"></a>

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>Si has instalado Windows Admin Center como una **puerta de enlace en Windows Server**

* ¿Se actualiza desde una versión anterior de Windows Admin Center? Compruebe que no se eliminó la regla de firewall debido a [este problema conocido](known-issues.md#upgrade). Utilice el siguiente comando de PowerShell para determinar si existe la regla. Si no, siga [estas instrucciones](known-issues.md#upgrade) para volver a crearla.
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Comprueba la versión de Windows](#winvercompat) del cliente y del servidor.

* Asegúrate de que utilizas Microsoft Edge o Google Chrome como navegador web.

* En el servidor, abra el Administrador de tareas > Servicios y asegúrese de que **ServerManagementGateway / Windows Admin Center** se está ejecutando.
![](../media/Service-TaskMan.PNG)

* Probar la conexión de red a la puerta de enlace (reemplace \<valores > con la información de la implementación)
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[volver al principio]](#toc)

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>Si ha instalado Windows Admin Center en una máquina virtual de Azure Windows Server

* [Comprobar la versión de Windows](#winvercompat)
* ¿Has agregado una regla de puerto entrante para HTTPS? 
* [Más información acerca de cómo instalar Windows Admin Center en una máquina virtual de Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[volver al principio]](#toc)

<a id="winvercompat"></a>

### <a name="check-the-windows-version"></a>Comprobar la versión de Windows

* Abre el cuadro de diálogo Ejecutar (tecla de Windows + R) e inicia ```winver```.

* Si estás usando Windows 10 versión 1703 o anterior, Windows Admin Center no se admite en su versión de Microsoft Edge. Puedes actualizar a una versión reciente de Windows 10 o usar Chrome.

* Si usa una versión preliminar de insider de Windows 10 o Server con una versión de compilación entre 17134 y 17637, Windows tenían un error que causaba Windows Admin Center producir un error. Use una versión compatible actual de Windows.

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>Asegúrese de que el servicio Administración remota de Windows (WinRM) se está ejecutando en la máquina de puerta de enlace y el nodo administrado

* Abra el cuadro de diálogo Ejecutar con Windows+1 + R
* Tipo ```services.msc``` y presione ENTRAR
* En la ventana que se abre, busque la administración remota de Windows (WinRM), asegúrese de que se está ejecutando y configurado para iniciarse automáticamente

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>¿Actualizar el servidor de 2016 para 2019?

* Esto es posible que haya desactivado la configuración de hosts de confianza. [Siga estas instrucciones para actualizar la configuración de hosts de confianza.](#configure-trustedhosts) 

[[volver al principio]](#toc)

<a id="tls"></a>

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>Puede obtener el mensaje: "No se puede conectar de forma segura a esta página. Puede que el sitio utiliza la configuración de seguridad TLS obsoleta o no seguras.

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
El equipo está restringido a las conexiones HTTP/2. Windows Admin Center usa la autenticación integrada de Windows, lo que no se admite en HTTP/2. Agregue los dos valores siguientes en el ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` tecla **el equipo que ejecuta el navegador** para quitar la restricción de HTTP/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[volver al principio]](#toc)

<a id="websockets"></a> 

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>Tengo problemas con las herramientas de escritorio remoto, eventos y PowerShell.

Estas tres herramientas requieren el protocolo websocket, que normalmente está bloqueado por los servidores proxy y firewalls. Si usa Google Chrome, hay un [problema conocido](known-issues.md#google-chrome) con websockets y la autenticación NTLM.

[[volver al principio]](#toc)


<a id="connectionissues"></a> 

## <a name="i-can-connect-to-some-servers-but-not-others"></a>Puedo conectar con algunos servidores, pero no con otros
* Inicie sesión en el equipo de puerta de enlace local e intente ```Enter-PSSession <machine name>``` en PowerShell, reemplazando \<nombre del equipo > con el nombre de la máquina que desea administrar en Windows Admin Center. 

* Si tu entorno utiliza un grupo de trabajo en lugar de un dominio, consulta [Estoy usando Windows Admin Center en un grupo de trabajo](#workgroup).

* **Uso de cuentas de administrador local:** Si usas una cuenta de usuario local que no es la cuenta de administrador integrada, deberá habilitar la directiva en el equipo de destino ejecutando el siguiente comando en PowerShell o en un símbolo del sistema como administrador en el equipo de destino:

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[volver al principio]](#toc)

<a id="workgroup"></a>

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

    > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Establece TrustedHosts en NetBIOS, IP o el FQDN de los equipos que desees administrar:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

    > [!TIP] 
    >Para una forma sencilla de establecer TrustedHosts enseguida, puedes usar un carácter comodín.

    >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. Cuando hayas terminado las pruebas, puedes emitir el comando siguiente desde una sesión de PowerShell con privilegios elevados para borrar la configuración de TrustedHosts:

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Si anteriormente habías exportado la configuración, abre el archivo, copia los valores y usa este comando:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

[[volver al principio]](#toc)

<a id="urlacl"></a>

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>Windows Admin Center instalado previamente, y ahora nada puede usar el mismo puerto TCP/IP

Ejecutar manualmente estos dos comandos en un símbolo del sistema con privilegios elevados:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[volver al principio]](#toc)

<a id="azlogin"></a>

## <a name="im-having-issues-using-azure-features-in-edge"></a>Tengo problemas con las características de Azure de Edge

Borde tiene [problemas conocidos](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) relacionados con las zonas de seguridad que afectan al inicio de sesión de Azure en Windows Admin Center. Si tiene problemas para usar las características de Azure al usar Edge, pruebe a agregar https://login.microsoftonline.com, https://login.live.com y la dirección URL de la puerta de enlace como sitios de confianza y a los sitios permitidos para la configuración del bloqueador de elementos emergentes de Edge en el explorador del lado cliente. 

Para ello:
1. Busque **opciones de Internet** en el Windows menú Inicio
2. Vaya a la **seguridad** ficha
3. En el **sitios de confianza** opción, haga clic en el **sitios** botón y agregue las direcciones URL en el cuadro de diálogo que aparece. Deberá agregar la dirección URL de puerta de enlace, así como https://login.microsoftonline.com y https://login.live.com.
4. Vaya a la **privacidad** ficha
5. En el **Bloqueador de elementos emergentes** sección, haga clic en el **configuración** botón y agregue las direcciones URL en el cuadro de diálogo que aparece. Deberá agregar la dirección URL de puerta de enlace, así como https://login.microsoftonline.com y https://login.live.com.


[[volver al principio]](#toc)

<a id="azissue"></a>

## <a name="having-an-issue-with-an-azure-related-feature"></a>¿Tiene un problema con una característica de Azure?

Por favor, envíenos un correo electrónico a wacFeedbackAzure@microsoft.com con la siguiente información:
* Información general del problema de la [preguntas que se enumeran a continuación](#filebug). 
* Describa el problema y los pasos llevados a cabo para reproducir el problema. 
* ¿Previamente registrar la puerta de enlace en Azure mediante el script descargable AadApp.ps1 de nuevo y, a continuación, actualice a la versión 1807? ¿O se ha registrado la puerta de enlace mediante la interfaz de usuario de configuración de puerta de enlace de Azure > Azure?
* ¿Es la cuenta de Azure asociada a varios directorios o inquilinos?
    * Si es así: ¿Al registrar la aplicación de Azure AD para Windows Admin Center, era el directorio que usa el directorio predeterminado en Azure? 
* ¿La cuenta de Azure tiene acceso a varias suscripciones?
* ¿Tiene la suscripción que estaba usando adjunta la facturación?
* ¿Inició a varias cuentas de Azure cuando se produjo el problema?
* ¿La cuenta de Azure requiere la autenticación multifactor?
* ¿Es el equipo que está intentando administrar una máquina virtual de Azure?
* ¿Windows Admin Center está instalado en una máquina virtual de Azure?

[[volver al principio]](#toc)

<a id="filebug"></a>

## <a name="still-not-working-or-is-your-issue-not-captured-here-troubleshooting-common-questions"></a>¿Todavía no funciona, o está el problema no mencionado aquí? [solución de problemas de las preguntas más frecuentes]

Ve a Visor de sucesos > Aplicaciones y servicios > Microsoft-ServerManagementExperience y busca errores o advertencias.

Archiva un error en nuestro [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) que describa el problema.

No olvides incluir errores o advertencias que encuentres en el registro de eventos, así como la siguiente información: 

* Plataforma donde está **instalado** Windows Admin Center (Windows 10 o Windows Server):
    * Si instala en el servidor, ¿cuál es el Windows [versión](#winvercompat) de **el equipo que ejecuta el navegador** para tener acceso a Windows Admin Center: 
    * ¿Está utilizando el certificado autofirmado creado por el instalador?
    * ¿Si estás utilizando tu propio certificado, ¿coincide el nombre del firmante con el equipo?
    * Si está utilizando tu propio certificado,¿especifica un nombre de firmante alternativo?
* ¿Has instalado con la configuración de puerto predeterminada?
    * Si no es así, ¿qué puerto especificaste?
* ¿Está el equipo donde está **instalado** Windows Admin Center unido a un dominio?
* Windows [versión](#winvercompat) donde está **instalado** Windows Admin Center:
* ¿Está el equipo que estás **intentando administrar** unido a un dominio?
* Windows [versión](#winvercompat) de la máquina que estás **intentando administrar**:
* ¿Qué explorador utilizas?
    * Si estás utilizando Google Chrome, ¿qué versión? (Ayuda > Acerca de Google Chrome)

[[volver al principio]](#toc)