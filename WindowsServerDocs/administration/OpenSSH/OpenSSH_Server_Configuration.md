---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH;SSH;SSHD;install;setup;instalación;configuración
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configuración del servidor de OpenSSH para Windows
ms.openlocfilehash: 5eb3d86950d169fd01512d330f0c04669beeffae
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259048"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuración del servidor de OpenSSH para Windows 10, 1809 y Windows Server 2019

En este tema se trata la configuración específica de Windows para el servidor OpenSSH (sshd). 

OpenSSH mantiene documentación detallada sobre las opciones de configuración en línea en [OpenSSH.com](https://www.openssh.com/manual.html) que no está duplicada en este conjunto de documentación. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configuración del shell predeterminado para OpenSSH en Windows

El shell de comandos predeterminado proporciona la experiencia que un usuario ve al conectarse al servidor mediante SSH. La primera ventana predeterminada es el shell de comandos de Windows (cmd.exe). Windows también incluye PowerShell y Bash, y los shells de comandos de terceros también están disponibles para Windows y pueden configurarse como el shell predeterminado para un servidor.

Para establecer el shell de comandos predeterminado, primero confirma que la carpeta de instalación OpenSSH está en la ruta de acceso del sistema. En Windows, la carpeta de instalación predeterminada es SystemDrive:WindowsDirectory\System32\openssh. Los siguientes comandos muestran la configuración de la ruta de acceso actual y le agregan la carpeta de instalación de OpenSSH predeterminada. 

Shell de comandos | Comando que se va a usar
------------- | -------------- 
Comando | path
PowerShell | $env:path

La configuración del shell de SSH predeterminado se realiza en el registro de Windows. Para ello, se agrega la ruta de acceso completa al ejecutable del shell a Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH en el valor de cadena DefaultShell. 

Como ejemplo, el siguiente comando de PowerShell establece PowerShell.exe como shell predeterminado:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Configuraciones de Windows en sshd_config 

En Windows, sshd lee los datos de configuración de %programdata%\ssh\sshd_config de forma predeterminada, aunque puedes especificar un archivo de configuración diferente si inicias sshd.exe con el parámetro -f.
Si el archivo no está presente, sshd genera uno con la configuración predeterminada cuando se inicia el servicio.

Los elementos que se enumeran a continuación proporcionan una configuración específica de Windows a través de entradas en sshd_config. Hay otras opciones de configuración posibles que no se mencionan aquí, ya que se describen a detalle en la [documentación de Win32 OpenSSH](https://github.com/powershell/win32-openssh/wiki) en línea. 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Puedes controlar qué usuarios y grupos se pueden conectar al servidor mediante las directivas AllowGroups, AllowUsers, DenyGroups y DenyUsers. Las directivas para permitir o denegar se procesan en el siguiente orden: DenyUsers, AllowUsers, DenyGroups y por último AllowGroups. Todos los nombres de cuenta deben especificarse en minúsculas. Consulta PATRONES en ssh_config para obtener más información sobre los patrones para los caracteres comodín.

Al configurar reglas basadas en grupos o usuarios con un usuario o grupo de dominio, usa el formato siguiente: ``` user?domain* ```.
Windows admite varios formatos para especificar las entidades de seguridad de dominio, pero muchas presentan problemas con los patrones estándar de Linux. Por ese motivo, se agrega * para cubrir los nombres de dominio completos. Además, este enfoque usa "?" en lugar de "\@" para evitar conflictos con el formato username@host. 

Los usuarios y grupos del grupo de trabajo y las cuentas conectadas a Internet siempre se resuelven en el nombre de cuenta local (sin la parte del dominio, de forma similar a los nombres estándar de UNIX). Los usuarios y grupos de dominio se resuelven estrictamente en el formato [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format): nombre_corto_dominio\nombre_usuario. Todas las reglas de configuración basadas en usuarios o grupos deben usar este formato.

Ejemplos de usuarios y grupos de dominio 

```
DenyUsers contoso\admin@192.168.2.23 : blocks contoso\admin from 192.168.2.23
DenyUsers contoso\* : blocks all users from contoso domain
AllowGroups contoso\sshusers : only allow users from contoso\sshusers group
```

Ejemplos de usuarios y grupos locales 

```
AllowUsers localuser@192.168.2.23
AllowGroups sshusers
```

### <a name="authenticationmethods"></a>AuthenticationMethods 

En Windows OpenSSH, los únicos métodos de autenticación disponibles son "password" y "publickey".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

El valor predeterminado es ".ssh/authorized_keys .ssh/authorized_keys2". Si la ruta de acceso no es absoluta, se crea de forma relativa con el directorio principal del usuario (o la ruta de acceso de la imagen de perfil). Por ejemplo: c:\users\usuario.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (compatibilidad agregada en la versión 7.7.0.0)

Esta directiva solo es compatible con las sesiones SFTP. Una sesión remota en cmd.exe no admitirá la directiva. Para configurar un servidor de chroot solo para SFTP, establece ForceCommand en internal-sftp. También puedes configurar SCP con chroot si implementas un shell personalizado que solo permita SCP y SFTP.

### <a name="hostkey"></a>HostKey

Los valores predeterminados son %programdata%/ssh/ssh_host_ecdsa_key, %programdata%/ssh/ssh_host_ed25519_key, %programdata%/ssh/ssh_host_dsa_key y %programdata%/ssh/ssh_host_rsa_key. Si los valores predeterminados no están presentes, sshd los genera automáticamente durante un inicio del servicio.

### <a name="match"></a>Coincidencia

Ten en cuenta las reglas de patrón en esta sección. Los nombres de usuario y grupo deben estar en minúsculas.

### <a name="permitrootlogin"></a>PermitRootLogin

No se aplica en Windows. Para evitar el inicio de sesión de administrador, usa a los administradores con la directiva DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Si necesitas un registro basado en archivos, usa LOCAL0. Los registros se generan en %programdata%\ssh\logs.
Cualquier otro valor, incluido el valor predeterminado AUTH, dirige los registros a ETW. Para obtener más información, consulta Instalaciones de registro en Windows.

### <a name="not-supported"></a>Incompatible 

Las siguientes opciones de configuración no están disponibles en la versión de OpenSSH que se incluye con Windows Server 2019 y Windows 10, 1809:

* AcceptEnv
* AllowStreamLocalForwarding
* AuthorizedKeysCommand
* AuthorizedKeysCommandUser
* AuthorizedPrincipalsCommand
* AuthorizedPrincipalsCommandUser
* Compresión
* ExposeAuthInfo
* GSSAPIAuthentication
* GSSAPICleanupCredentials
* GSSAPIStrictAcceptorCheck
* HostbasedAcceptedKeyTypes
* HostbasedAuthentication
* HostbasedUsesNameFromPacketOnly
* IgnoreRhosts
* IgnoreUserKnownHosts
* KbdInteractiveAuthentication
* KerberosAuthentication
* KerberosGetAFSToken
* KerberosOrLocalPasswd
* KerberosTicketCleanup
* PermitTunnel
* PermitUserEnvironment
* PermitUserRC
* PidFile
* PrintLastLog
* RDomain
* StreamLocalBindMask
* StreamLocalBindUnlink
* StrictModes
* X11DisplayOffset
* X11Forwarding
* X11UseLocalhost
* XAuthLocation

