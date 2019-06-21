---
title: Administración de protocolos SSL/TLS y conjuntos de cifrado de AD FS
description: Preguntas más frecuentes para AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c1153cd81185dcfe83d291161a85481e5a7d0700
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280549"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Administración de protocolos SSL/TLS y conjuntos de cifrado de AD FS
La siguiente documentación proporciona información sobre cómo deshabilitar y habilitar ciertos protocolos TLS/SSL y conjuntos que son usados por AD FS de cifrado

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel como conjuntos de cifrado en AD FS

La capa de transporte (TLS) y capa de Sockets seguros (SSL) son protocolos que proporcionan para proteger las comunicaciones.  Servicios de federación de Active Directory utiliza estos protocolos de comunicaciones.  Hoy en día existen varias versiones de estos protocolos.

Schannel es un proveedor de compatibilidad para seguridad (SSP) que implementa los protocolos de autenticación estándar de Internet de SSL y TLS y DTLS. La interfaz del proveedor de compatibilidad para seguridad (SSPI) es una API que usan los sistemas Windows para realizar funciones basadas en la seguridad, incluida la autenticación. SSPI funciona como una interfaz común a varios proveedores de compatibilidad para seguridad (SSP), incluido el Schannel SSP.

Un conjunto de cifrado es un conjunto de algoritmos criptográficos. La implementación de SSP de schannel de los protocolos TLS/SSL usan algoritmos de un conjunto de cifrado para crear claves y cifrar la información. Un conjunto de cifrado especifica un algoritmo para cada una de las siguientes tareas:

- Intercambio de claves
- Cifrado masivo
- Autenticación de mensajes

AD FS usa Schannel.dll llevar a cabo sus interacciones de las comunicaciones seguras.  Actualmente, AD FS admite todos los protocolos y los conjuntos de cifrado que son compatibles con el archivo Schannel.dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Administración de los protocolos TLS/SSL y conjuntos de cifrado
> [!IMPORTANT]
> Esta sección contiene los pasos que indican cómo modificar el registro. Sin embargo, pueden producirse problemas graves si modifica incorrectamente el registro. Por lo tanto, asegúrese de que sigue estos pasos cuidadosamente. 
> 
> Tenga en cuenta que va a cambiar la configuración predeterminada de seguridad de SCHANNEL podría interrumpir o impedir la comunicación entre determinados servidores y clientes.  Esto ocurrirá si se requiere una comunicación segura y no tiene un protocolo para negociar comunicaciones con.
> 
> Si va a aplicar estos cambios, se debe aplicar a todos los servidores de AD FS en la granja de servidores.  Después de aplicar estos cambios, es necesario reiniciar.

En el día de hoy y edad, reforzar sus servidores y quitar anteriores o conjuntos de cifrado débil se está convirtiendo en una mayor prioridad para muchas organizaciones.  Están disponibles los paquetes de software que probará los servidores y proporcionan información detallada sobre estos protocolos y los conjuntos.  Con el fin de mantener la conformidad o lograr las clasificaciones de seguras, quitar o deshabilitar los protocolos más débiles o conjuntos de cifrado se ha convertido en un requisito indispensable.  El resto de este documento le proporcionará instrucciones sobre cómo habilitar o deshabilitar ciertos protocolos y conjuntos de cifrado.

Las claves del registro siguientes se encuentran en la misma ubicación:  HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Utilice regedit o PowerShell para habilitar o deshabilitar estos protocolos y conjuntos de cifrado.

![Ubicación del Registro](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Habilitar y deshabilitar SSL 2.0
Para habilitar y deshabilitar SSL 2.0, utilice las siguientes claves del registro y sus valores.

### <a name="enable-ssl-20"></a>Habilitar SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault"=dword:00000000 

### <a name="disable-ssl-20"></a>Deshabilitar SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault"=dword:00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault"=dword:00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Uso de PowerShell para deshabilitar SSL 2.0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Habilitar y deshabilitar SSL 3.0
Para habilitar y deshabilitar SSL 3.0, utilice las siguientes claves del registro y sus valores.

### <a name="enable-ssl-30"></a>Habilitar SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault"=dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault"=dword:00000000 

### <a name="disable-ssl-30"></a>Deshabilitar SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault"=dword:00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Uso de PowerShell para deshabilitar SSL 3.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>Habilitar y deshabilitar TLS 1.0
Para habilitar y deshabilitar TLS 1.0, use las siguientes claves del registro y sus valores.

> [!IMPORTANT]
> Deshabilitación de TLS 1.0 se interrumpirá el WAP para confianza de AD FS.  Si deshabilita TLS 1.0 debe habilitar la autenticación segura para sus aplicaciones.  Consulte [habilitar la autenticación sólida](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Habilitar TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault"=dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault"=dword:00000000 

### <a name="disable-tls-10"></a>Deshabilitación de TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault"=dword:00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Uso de PowerShell para deshabilitar TLS 1.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>Habilitar y deshabilitar TLS 1.1
Utilice las siguientes claves del registro y sus valores para habilitar y deshabilitar TLS 1.1.

### <a name="enable-tls-11"></a>Habilitar TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault"=dword:00000000

### <a name="disable-tls-11"></a>Deshabilitación de TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault"=dword:00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Uso de PowerShell para deshabilitar TLS 1.1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>Habilitar y deshabilitar TLS 1.2

Utilice las siguientes claves del registro y sus valores para habilitar y deshabilitar TLS 1.2.

### <a name="enable-tls-12"></a>Habilitar TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000

### <a name="disable-tls-12"></a>Deshabilitación de TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000001

### <a name="using-powershell-to-disable-tls-12"></a>Uso de PowerShell para deshabilitar TLS 1.2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>Habilitar y deshabilitar RC4 

Utilice las siguientes claves del registro y sus valores para habilitar y deshabilitar RC4.  Claves de registro de este conjunto de cifrado se encuentran aquí:

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Ubicación del Registro](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Habilitar RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled"=dword:00000001 

### <a name="disable-rc4"></a>Deshabilitar RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled"=dword:00000000 

### <a name="using-powershell"></a>Con PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Habilitar o deshabilitar los conjuntos de cifrado adicionales

Puede deshabilitar ciertos cifrados específicos quitándolas de HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 

![Ubicación del Registro](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Para habilitar un conjunto de cifrado, agregue su valor de cadena a la clave del valor de cadena múltiple de funciones.  Por ejemplo, si queremos habilitar TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521, a continuación, se agregaría, a la cadena.

Para obtener una lista completa de cifrado compatibles consulte conjuntos [conjuntos de cifrado en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).  Este documento proporciona una tabla de los conjuntos que están habilitadas de forma predeterminada y los que son compatibles pero no habilitado de forma predeterminada.  Para dar prioridad a los conjuntos de cifrado, consulte [dando prioridad a los conjuntos de cifrado de Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Al habilitar la autenticación segura para las aplicaciones .NET
Las aplicaciones de .NET Framework 3.5/4.0/4.5.x pueden cambiar el protocolo predeterminado a TLS 1.2 habilitando la clave del registro SchUseStrongCrypto.  Esta clave del registro obligará a aplicaciones .NET para usar TLS 1.2.

> [!IMPORTANT]
> Deberá usar la clave de .NET Framework 4.0 o 4.5. x para AD FS en Windows Server 2016 y Windows Server 2012 R2:  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319


Para .NET Framework 3.5, use la siguiente clave del registro:

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\.NETFramework\v2.0.50727] "SchUseStrongCrypto"=dword:00000001

Para .NET Framework 4.0 o 4.5. x use la siguiente clave del registro: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319 "SchUseStrongCrypto"=dword:00000001

![Autenticación segura](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Información adicional

- [Conjuntos de cifrado en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)
- [Conjuntos de cifrado TLS en Windows 8.1](https://msdn.microsoft.com/library/windows/desktop/mt767781.aspx)
- [Dar prioridad a los conjuntos de cifrado de Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)
- [Hablando en cifrados y otro lengua Enigmatic](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
