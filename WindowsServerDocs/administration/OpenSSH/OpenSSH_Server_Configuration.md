---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalar, configurar
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configuración del servidor de OpenSSH para Windows
ms.openlocfilehash: 8e6476e4005bd5bbc2d40c8a59d39510ca1beb70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827286"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuración del servidor de OpenSSH para Windows 10 1809 y Server 2019#

Este tema explica la configuración específica de Windows para el servidor de OpenSSH (sshd). 

OpenSSH mantiene documentación detallada sobre las opciones de configuración en línea en [OpenSSH.com](https://www.openssh.com/manual.html), que no es se duplican en este conjunto de documentación. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configurar el shell predeterminado de OpenSSH en Windows

El shell de comandos predeterminada proporciona la experiencia de que un usuario ve cuando se conecta al servidor mediante SSH. El valor predeterminado inicial Windows es el shell de comandos de Windows (cmd.exe). También incluyen Windows PowerShell y Bash y shells de comandos de otros fabricantes también están disponibles para Windows y se pueden configurar como el shell predeterminado para un servidor.

Para establecer el valor predeterminado de shell de comandos, primero confirme que la carpeta de instalación de OpenSSH está en la ruta de acceso del sistema. Para Windows, la carpeta de instalación predeterminada es SystemDrive:WindowsDirectory\System32\openssh. Los siguientes comandos se muestra la configuración actual de la ruta de acceso y agregar la carpeta de instalación predeterminada OpenSSH a él. 

Shell de comandos | Comando para usar
------------- | -------------- 
Comando | ruta de acceso
PowerShell | $env:\path

Configurar el valor predeterminado de ssh shell se realiza en el registro de Windows mediante la adición de la ruta de acceso completa al ejecutable en el valor de cadena DefaultShell Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH shell. 

Por ejemplo, el siguiente comando de Powershell establece el shell predeterminado sea PowerShell.exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshdconfig"></a>Configuraciones de Windows en sshd_config 

En Windows, sshd lee datos de configuración de %programdata%\ssh\sshd_config de forma predeterminada, o se puede especificar un archivo de configuración diferente, inicie sshd.exe con el parámetro -f.
Si el archivo está presente, sshd genera uno con la configuración predeterminada cuando se inicia el servicio.

Los elementos enumerados a continuación proporcionan posibles de configuración específicos de Windows a través de las entradas de sshd_config. Hay otras opciones de configuración posibles en que no se muestran aquí, ya que se tratan detalladamente en online [documentación Win32 OpenSSH](https://github.com/powershell/win32-openssh/wiki). 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Controlar qué usuarios y grupos pueden conectarse al servidor se realiza mediante las directivas AllowGroups, AllowUsers, DenyGroups y DenyUsers. Las directivas de permitir o denegar se procesan en el orden siguiente: DenyUsers, AllowUsers, DenyGroups y finalmente AllowGroups. Todos los nombres de cuenta deben especificarse en minúsculas. Ver patrones en ssh_config para obtener más información sobre los patrones de caracteres comodín.

Cuando la configuración de usuario o grupo en función de las reglas con un usuario de dominio o grupo, use el siguiente formato: ``` user?domain* ```.
Windows permite varios formatos para especificar las entidades de dominio, pero muchos entran en conflicto con el patrón estándar de Linux. Por esta razón, * se agregan para cubrir el FQDN. Además, este enfoque utiliza "?", en lugar de @, para evitar conflictos con los username@host formato. 

Grupo de trabajo a los usuarios o grupos y cuentas conectadas a internet siempre se resuelven en su nombre de cuenta local (ninguna parte del dominio, similar a los nombres de Unix estándares). Los usuarios del dominio y los grupos son estrictamente resuelve en [NameSamCompatible](https://docs.microsoft.com/en-us/windows/desktop/api/secext/ne-secext-extended_name_format) formato - domain_short_name\user_name. Todos los usuarios y grupos según las reglas deben adherirse a este formato de configuración.

Ejemplos de los usuarios del dominio y grupos 

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

Para Windows OpenSSH, los métodos de autenticación solo está disponible son "password" y "publickey".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

El valor predeterminado es ".ssh/authorized_keys .ssh/authorized_keys2". Si la ruta de acceso no es absoluta, se obtiene con respecto al directorio principal del usuario (o ruta de acceso de imagen de perfil). P. ej. c:\users\user.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (compatibilidad agregada en v7.7.0.0)

Esta directiva solo es compatible con las sesiones de sftp. Una sesión remota en cmd.exe no respeta este. Para configurar un servidor sftp solo chroot, establézcalo ForceCommand interno sftp. También puede configurar scp con chroot, si implementa un shell personalizado que sólo se permitiría scp y sftp.

### <a name="hostkey"></a>HostKey

Los valores predeterminados son % programdata%/ssh/ssh_host_ecdsa_key, %programdata%/ssh/ssh_host_ed25519_key y % programdata%/ssh/ssh_host_rsa_key. Si los valores predeterminados no están presentes, sshd genera automáticamente en un inicio de servicio.

### <a name="match"></a>Coincidencia

Tenga en cuenta que las reglas en esta sección de patrón. Los nombres de usuario y grupo deben estar en minúsculas.

### <a name="permitrootlogin"></a>PermitRootLogin

No es aplicable en Windows. Para evitar el inicio de sesión de administrador, utilice los administradores con DenyGroups directiva.

### <a name="syslogfacility"></a>SyslogFacility

Si necesita un archivo basado en registro, use LOCAL0. Los registros se generan bajo % programdata%\ssh\logs.
Cualquier otro valor, incluido el valor predeterminado AUTH dirige el registro de ETW. Para obtener más información, consulte las funciones de registro en Windows.

### <a name="not-supported"></a>No se admite 

Las siguientes opciones de configuración no están disponibles en la versión de OpenSSH en el que se incluye en Windows Server 2019 y Windows 10 1809:

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

