---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalación y configuración
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configuración del servidor OpenSSH para Windows
ms.openlocfilehash: ed424c33c4cd2c19a9b5e985ab6083bcbcb9fbdc
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546264"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuración del servidor OpenSSH para Windows 10 1809 y Server 2019

En este tema se trata la configuración específica de Windows para el servidor OpenSSH (sshd). 

OpenSSH mantiene documentación detallada sobre las opciones de configuración en línea en [OpenSSH.com](https://www.openssh.com/manual.html), que no se duplica en este conjunto de documentación. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configuración del shell predeterminado para OpenSSH en Windows

El shell de comandos predeterminado proporciona la experiencia que un usuario ve al conectarse al servidor mediante SSH. La primera ventana predeterminada es el shell de comandos de Windows (cmd. exe). Windows también incluye PowerShell y Bash, y los shells de comandos de terceros también están disponibles para Windows y pueden configurarse como el shell predeterminado para un servidor.

Para establecer el shell de comandos predeterminado, primero confirme que la carpeta de instalación OpenSSH está en la ruta de acceso del sistema. En Windows, la carpeta de instalación predeterminada es SystemDrive: WindowsDirectory\System32\openssh. Los siguientes comandos muestran la configuración de la ruta de acceso actual y le agregan la carpeta de instalación de OpenSSH predeterminada. 

Shell de comandos | Comando que se va a usar
------------- | -------------- 
Comando | path
PowerShell | $env:p gistro

La configuración del shell de ssh predeterminado se realiza en el registro de Windows agregando la ruta de acceso completa al ejecutable de Shell en Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH en el valor de cadena DefaultShell. 

Como ejemplo, el siguiente comando de PowerShell establece el shell predeterminado para que sea PowerShell. exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Configuraciones de Windows en sshd_config 

En Windows, sshd Lee los datos de configuración de%ProgramData%\ssh\sshd_config de forma predeterminada, o se puede especificar un archivo de configuración diferente iniciando sshd. exe con el parámetro-f.
Si el archivo no está presente, sshd genera uno con la configuración predeterminada cuando se inicia el servicio.

Los elementos que se enumeran a continuación proporcionan una configuración específica de Windows posible a través de las entradas de sshd_config. Hay otras opciones de configuración posibles en que no se enumeran aquí, ya que se explican detalladamente en la [documentación de Win32 OpenSSH](https://github.com/powershell/win32-openssh/wiki)en línea. 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

El control de los usuarios y grupos que se pueden conectar al servidor se realiza mediante las directivas AllowGroups, AllowUsers, DenyGroups y DenyUsers. Las directivas de permiso y denegación se procesan en el orden siguiente: DenyUsers, AllowUsers, DenyGroups y Finally AllowGroups. Todos los nombres de cuenta deben especificarse en minúsculas. Vea patrones en ssh_config para obtener más información sobre los patrones de caracteres comodín.

Al configurar reglas basadas en grupos o usuarios con un usuario o grupo de dominio, use el siguiente ``` user?domain* ```formato:.
Windows permite varios formatos para especificar entidades de seguridad de dominio, pero muchos conflictos con patrones estándar de Linux. Por ese motivo, se agrega * para cubrir los FQDN. Además, este enfoque usa "?", en lugar de @, para evitar conflictos con el username@host formato. 

Los usuarios y grupos del grupo de trabajo y las cuentas conectadas a Internet siempre se resuelven en su nombre de cuenta local (sin parte del dominio, de forma similar a los nombres UNIX estándar). Los usuarios y grupos de dominio se resuelven estrictamente en formato [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) -domain_short_name\user_name. Todas las reglas de configuración basadas en usuario/grupo deben cumplir este formato.

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

En Windows OpenSSH, los únicos métodos de autenticación disponibles son "Password" y "publickey".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

El valor predeterminado es ". ssh/authorized_keys. ssh/authorized_keys2". Si la ruta de acceso no es absoluta, se toma en relación con el directorio principal del usuario (o la ruta de acceso de la imagen de perfil). Antiguo. c:\Users\nombre.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (compatibilidad agregada en v 7.7.0.0)

Esta directiva solo se admite con sesiones SFTP. Una sesión remota en cmd. exe no lo admitirá. Para configurar un servidor de chroot solo SFTP, establezca ForceCommand en Internal-SFTP. También puede configurar SCP con chroot, implementando un shell personalizado que solo permita SCP y SFTP.

### <a name="hostkey"></a>HostKey

Los valores predeterminados son% ProgramData%/ssh/ssh_host_ecdsa_key,% ProgramData%/ssh/ssh_host_ed25519_key y% ProgramData%/ssh/ssh_host_rsa_key. Si los valores predeterminados no están presentes, sshd los genera automáticamente en un inicio de servicio.

### <a name="match"></a>Coincidencia

Tenga en cuenta las reglas de patrón en esta sección. Los nombres de usuario y grupo deben estar en minúsculas.

### <a name="permitrootlogin"></a>PermitRootLogin

No es aplicable en Windows. Para evitar el inicio de sesión de administrador, use los administradores con la Directiva DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Si necesita un registro basado en archivos, use LOCAL0. Los registros se generan en%ProgramData%\ssh\logs.
Cualquier otro valor, incluido el valor predeterminado AUTH, dirige el registro a ETW. Para obtener más información, consulte registro de instalaciones en Windows.

### <a name="not-supported"></a>No compatible 

Las siguientes opciones de configuración no están disponibles en la versión OpenSSH que se incluye en Windows Server 2019 y Windows 10 1809:

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

