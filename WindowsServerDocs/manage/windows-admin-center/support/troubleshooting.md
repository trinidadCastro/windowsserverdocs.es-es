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
ms.openlocfilehash: a91a8dcf6f05ef0ef66dee603851150b2145d559
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297122"
---
# Solución de problemas de Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

> [!Important]
> Esta guía te ayudará a diagnosticar y resolver problemas que te impiden usar Windows Admin Center. Si tienes un problema con una herramienta específica, comprueba si estás experimentando un [problema conocido.](http://aka.ms/wacknownissues)

<a id="toc"></a>

## Vínculos rápidos

* [Se produce un error en el programa de instalación con el mensaje: ** _no se pudo cargar el módulo de 'Microsoft.PowerShell.LocalAccounts'._**](#psmodulepath)

* Recibo un error del tipo **No se puede acceder a este sitio/página** en el navegador web (selecciona el tipo de implementación)
    * [Tengo instalado Windows Admin Center como una aplicación en Windows 10](#whitescreenw10)
    * [Tengo instalado Windows Admin Center como una puerta de enlace en Windows Server](#whitescreenws)
    * [Tengo instalado Windows Admin Center como una puerta de enlace en una VM de Azure](#whitescreenazvm)

* [Carga de la página principal de Windows Admin Center, pero estoy bloqueado en el panel Agregar conexión o no puedo conectarme a ninguna máquina.](#winvercompat)

* [Aparece el mensaje: "Error al cargar el módulo. Rpc: reintentos de 'Ping' agotados."](#winvercompat)

* [Aparece el mensaje: "no se puede conectar de forma segura a esta página. Esto podría deberse a que el sitio utiliza la configuración de seguridad TLS obsoleto o no seguros."](#tls)

* [Tengo problemas con las herramientas de escritorio remoto, eventos y PowerShell.](#websockets)

* [Tengo problemas con las características de Azure de Edge](#azlogin)

* [Puedo conectar con algunos servidores, pero no con otros](#connectionissues)

* [Estoy usando Windows Admin Center en un **grupo de trabajo**](#workgroup)

* [Windows Admin Center instalado previamente y ahora nada más puede usar el mismo puerto TCP/IP](#urlacl)

* [Mi problema no se incluye aquí o los pasos de esta página no resuelven el problema.](#filebug)

<a id="psmodulepath"></a>

## Se produce un error en el programa de instalación con el mensaje: ** _no se pudo cargar el módulo de 'Microsoft.PowerShell.LocalAccounts'._**

Esto puede suceder si se ha modificado o eliminado la ruta de acceso de módulo de PowerShell de forma predeterminada. Para resolver el problema, debes asegurarte de que ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` es el **primer** elemento en la variable de entorno PSModulePath. Puedes lograrlo con la siguiente línea de PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## Recibo un error del tipo **No se puede acceder a este sitio/página** en el navegador web

<a id="whitescreenw10"></a>

### Si has instalado Windows Admin Center como una **aplicación en Windows 10**

* Comprueba que se ejecuta Windows Admin Center. Busca el icono de Windows Admin Center ![](../media/trayIcon.PNG) en la bandeja del sistema o **escritorio de Windows Admin Center / SmeDesktop.exe** en el Administrador de tareas. De otra forma, inicia **Windows Admin Center** en el menú Inicio.

> [!NOTE] 
> Después de reiniciar, debes iniciar Windows Admin Center desde el menú Inicio.  

* [Comprobar la versión de Windows](#winvercompat)

* Asegúrate de que utilizas Microsoft Edge o Google Chrome como navegador web.

* ¿Has seleccionado el certificado correcto en [¿primer inicio?](../use/get-started.md#selecting-a-client-certificate)

  * Intenta abrir el explorador en una sesión privada; si funciona, tendrás que borrar la memoria caché.

* ¿Recientemente actualizar Windows 10 a una nueva compilación o versión?

  * Esto es posible que haya desactivado la configuración de hosts de confianza. [Sigue estas instrucciones para actualizar la configuración de hosts de confianza.](#configure-trustedhosts) 

[[volver al inicio]](#toc)

<a id="whitescreenws"></a>

### Si has instalado Windows Admin Center como una **puerta de enlace en Windows Server**

* ¿Actualizar desde una versión anterior de Windows Admin Center? Comprueba que no se ha eliminado la regla de firewall debido a [este problema conocido](known-issues.md#upgrade). Usa el siguiente comando de PowerShell para determinar si existe la regla. De lo contrario, sigue [estas instrucciones](known-issues.md#upgrade) para volver a crearla.
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Comprueba la versión de Windows](#winvercompat) del cliente y del servidor.

* Asegúrate de que está utilizando Microsoft Edge o Google Chrome como navegador web.

* En el servidor, abra el Administrador de tareas > servicios y asegúrate de que **ServerManagementGateway / Windows Admin Center** se está ejecutando.
![](../media/Service-TaskMan.PNG)

* Probar la conexión de red a la puerta de enlace (reemplaza \<values> por la información de tu implementación)
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[volver al inicio]](#toc)

<a id="whitescreenazvm"></a>  

### Si has instalado Windows Admin Center en una MV de Windows Server de Azure

* [Comprobar la versión de Windows](#winvercompat)
* ¿Has agregado una regla de puerto entrante para HTTPS? 
* [Encontrarás más información acerca de cómo instalar Windows Admin Center en una VM de Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[volver al inicio]](#toc)

<a id="winvercompat"></a>

### Comprobar la versión de Windows

* Abre el cuadro de diálogo Ejecutar (tecla de Windows + R) e inicia ```winver```.

* Si estás usando Windows 10 versión 1703 o anterior, Windows Admin Center no se admite en su versión de Microsoft Edge. Puedes actualizar a una versión reciente de Windows 10 o usar Chrome.

* Si estás usando una versión preliminar de insider de Windows 10 o Server con una versión de compilación entre 17134 y 17637, Windows tenía un error que causó un error de Windows Admin Center. Usa una versión compatible actual de Windows.

### Asegúrese de que el servicio de administración remota de Windows (WinRM) se ejecuta en la máquina de puerta de enlace y el nodo administrado

* Abre el cuadro de diálogo de ejecución con Windows+1 + R
* Tipo de ```services.msc``` y presiona ENTRAR
* En la ventana que se abre, busca la administración remota de Windows (WinRM), asegúrate de que se está ejecutando y configurado para iniciarse automáticamente

### ¿Actualizar el servidor de 2016 a 2019?

* Esto es posible que haya desactivado la configuración de hosts de confianza. [Sigue estas instrucciones para actualizar la configuración de hosts de confianza.](#configure-trustedhosts) 

[[volver al inicio]](#toc)

<a id="tls"></a>

## Aparece el mensaje: "no se puede conectar de forma segura a esta página. Esto podría deberse a que el sitio utiliza la configuración de seguridad TLS obsoleto o no seguros.

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
El equipo está restringido a las conexiones HTTP/2. Windows Admin Center usa autenticación integrada de Windows, lo que no se admite en HTTP/2. Agrega los siguientes valores del dos registro bajo la ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` clave en **el equipo que ejecuta el navegador** para quitar la restricción de HTTP/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[volver al inicio]](#toc)

<a id="websockets"></a> 

## Tengo problemas con las herramientas de escritorio remoto, eventos y PowerShell.

Estas tres herramientas requieren el protocolo websocket, que normalmente está bloqueado por servidores proxy y firewalls. Si estás utilizando Google Chrome, hay un [problema conocido](known-issues.md#google-chrome) con websockets y la autenticación NTLM.

[[volver al inicio]](#toc)


<a id="connectionissues"></a> 

## Puedo conectar con algunos servidores, pero no con otros
* Inicia sesión localmente en el equipo de la puerta de enlace y prueba a ```Enter-PSSession <machine name>``` en PowerShell, reemplazando \<machine name> por el nombre de la máquina que estás intentando administrar en Windows Admin Center. 

* Si tu entorno utiliza un grupo de trabajo en lugar de un dominio, consulta [Estoy usando Windows Admin Center en un grupo de trabajo](#workgroup).

* **Con cuentas de administrador local:** si usas una cuenta de usuario local que no sea la cuenta de administrador integrada, debes habilitar la directiva en el equipo de destino ejecutando el siguiente comando en PowerShell o en un símbolo del sistema como administrador en el equipo de destino:

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[volver al inicio]](#toc)

<a id="workgroup"></a>

## Utilizar Windows Admin Center en un grupo de trabajo 

### ¿Qué cuenta estás utilizando?
Asegúrate de que las credenciales que usas son un miembro del grupo de administradores locales del servidor de destino. En algunos casos, WinRM también requiere la pertenencia a grupos en el grupo Usuarios de Administración remota. Si estás usando una cuenta de usuario local que **no sea la cuenta de administrador integrada**, tienes que habilitar la directiva en el equipo de destino ejecutando el siguiente comando en PowerShell o en un símbolo del sistema como Administrador en el equipo de destino:

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### ¿Estás conectando a un equipo de un grupo de trabajo en una subred diferente?

Para conectar a un equipo de grupo de trabajo que no esté en la misma subred que la puerta de enlace, asegúrate de que el puerto de firewall para WinRM (TCP 5985) permita el tráfico entrante en el equipo de destino. Puedes ejecutar el siguiente comando en PowerShell o en un símbolo del sistema como administrador en el equipo de destino para crear esta regla de firewall:

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### Configurar TrustedHosts

Al instalar Windows Admin Center, se proporciona la opción de permitir que Windows Admin Center administre la configuración de TrustedHosts de la puerta de enlace. Esto es necesario en un entorno de grupo de trabajo o cuando se usan credenciales de administrador local en un dominio. Si optas por pasar por alto esta configuración, debes configurar TrustedHosts manualmente.

**Para modificar TrustedHosts con los comandos de PowerShell:**

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

[[volver al inicio]](#toc)

<a id="urlacl"></a>

## Windows Admin Center instalado previamente y ahora nada más puede usar el mismo puerto TCP/IP

Ejecutar manualmente estas dos comandos en un símbolo del sistema con privilegios elevados:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[volver al inicio]](#toc)

<a id="azlogin"></a>

## Tengo problemas con las características de Azure de Edge

Edge tiene [problemas conocidos](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) relacionados con las zonas de seguridad que afectan al inicio de sesión de Azure en Windows Admin Center. Si tienes problemas al usar características de Azure al usar Edge, intenta agregar https://login.microsoftonline.com, https://login.live.com y la dirección URL de la puerta de enlace como sitios de confianza y a los sitios permitidos para la configuración del bloqueador de elementos emergentes de borde en el explorador del lado cliente. 

Para ello:
1. Búsqueda de **Opciones de Internet** en el menú de inicio de Windows
2. Ve a la pestaña de **seguridad**
3. En la opción de **Sitios de confianza** , haz clic en el botón **sitios** y agrega las direcciones URL en el cuadro de diálogo que se abre. Tendrás que agregar la dirección URL de la puerta de enlace, así como https://login.microsoftonline.com y https://login.live.com.
4. Ve a la pestaña de **privacidad**
5. En la sección **Bloqueador de elementos emergentes** , haz clic en el botón de **configuración** y agrega las direcciones URL en el cuadro de diálogo que se abre. Tendrás que agregar la dirección URL de la puerta de enlace, así como https://login.microsoftonline.com y https://login.live.com.


[[volver al inicio]](#toc)

<a id="azissue"></a>

## ¿Tienes un problema con una característica de Azure?

Envíanos un correo electrónico a wacFeedbackAzure@microsoft.com con la siguiente información:
* Información de problemas generales de las [preguntas que aparecen a continuación](#filebug). 
* Describir el problema y los pasos que tomó para reproducir el problema. 
* ¿Registrar previamente la puerta de enlace a Azure mediante el script descargable AadApp.ps1 de nuevo y, a continuación, actualizar a la versión 1807? O ¿registrar la puerta de enlace a Azure mediante la interfaz de usuario de configuración de puerta de enlace > Azure?
* ¿Es tu cuenta de Azure asociado con varios directorios o inquilinos?
    * ¿En caso afirmativo: al registrar la aplicación de Azure AD para Windows Admin Center, fue el directorio que se usó el directorio de forma predeterminada en Azure? 
* ¿Tu cuenta de Azure tiene acceso a varias suscripciones?
* ¿La suscripción que estabas usando tiene facturación conectada?
* ¿Se inició sesión varias cuentas de Azure cuando se produjo el problema?
* ¿Tu cuenta de Azure requiere la autenticación multifactor?
* ¿Es la máquina que estás intentando administrar una VM de Azure?
* ¿Está instalado Windows Admin Center en una VM de Azure?

[[volver al inicio]](#toc)

<a id="filebug"></a>

## ¿Aún no funciona, o el problema no aquí recogido? [solución de problemas de las preguntas más frecuentes]

Ve a Visor de sucesos > Aplicaciones y servicios > Microsoft-ServerManagementExperience y busca errores o advertencias.

Archiva un error en nuestro [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) que describa el problema.

No olvides incluir errores o advertencias que encuentres en el registro de eventos, así como la siguiente información: 

* Plataforma donde está **instalado** Windows Admin Center (Windows 10 o Windows Server):
    * Si instala en el servidor, ¿cuál es la [versión](#winvercompat) de Windows de **la máquina que ejecuta el navegador** para tener acceso a Windows Admin Center: 
    * ¿Si usas el certificado autofirmado creado por el instalador?
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

[[volver al inicio]](#toc)