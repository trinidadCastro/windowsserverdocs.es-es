---
title: Administración de los protocolos SSL/TLS y los conjuntos de cifrado para AD FS
description: Preguntas más frecuentes sobre AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e0c581a29db92cfb73e4225c72e7e1c2bad4ca68
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465285"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Administración de los protocolos SSL/TLS y los conjuntos de cifrado para AD FS
La siguiente documentación proporciona información sobre cómo deshabilitar y habilitar determinados protocolos TLS/SSL y conjuntos de cifrado que se usan en AD FS

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>Conjuntos de cifrado TLS/SSL, SChannel y Cipher en AD FS

La seguridad de la capa de transporte (TLS) y el Capa de sockets seguros (SSL) son protocolos que proporcionan comunicaciones seguras.  Servicios de federación de Active Directory (AD FS) usa estos protocolos para las comunicaciones.  Actualmente existen varias versiones de estos protocolos.

Schannel es un proveedor de compatibilidad para seguridad (SSP) que implementa los protocolos de autenticación estándar de Internet de SSL y TLS y DTLS. La interfaz del proveedor de compatibilidad para seguridad (SSPI) es una API que usan los sistemas Windows para realizar funciones basadas en la seguridad, incluida la autenticación. SSPI funciona como una interfaz común a varios proveedores de compatibilidad para seguridad (SSP), incluido el Schannel SSP.

Un conjunto de cifrado es un conjunto de algoritmos criptográficos. La implementación de Schannel SSP de los protocolos TLS/SSL usa algoritmos de un conjunto de cifrado para crear claves y cifrar la información. Un conjunto de cifrado especifica un algoritmo para cada una de las siguientes tareas:

- Intercambio de claves
- Cifrado masivo
- Autenticación de mensajes

AD FS usa Schannel. dll para realizar las interacciones de comunicaciones seguras.  Actualmente AD FS admite todos los protocolos y conjuntos de cifrado compatibles con Schannel. dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Administrar los protocolos TLS/SSL y los conjuntos de cifrado
> [!IMPORTANT]
> Esta sección contiene pasos que indican cómo modificar el registro. Sin embargo, pueden aparecer problemas graves si modifica el Registro incorrectamente. Por tanto, asegúrese de seguir estos pasos con cuidado. 
> 
> Tenga en cuenta que el cambio de la configuración de seguridad predeterminada para SCHANNEL podría interrumpir o impedir la comunicación entre determinados clientes y servidores.  Esto ocurrirá si se requiere una comunicación segura y no dispone de un protocolo para negociar las comunicaciones con.
> 
> Si va a aplicar estos cambios, deben aplicarse a todos los servidores de AD FS de la granja.  Después de aplicar estos cambios, es necesario reiniciar.

En el día y la edad actuales, la protección de los servidores y la eliminación de conjuntos de cifrado más antiguos o débiles se está convirtiendo en una prioridad importante para muchas organizaciones.  Hay conjuntos de software disponibles que prueban los servidores y proporcionan información detallada sobre estos protocolos y conjuntos.  A fin de mantener la compatibilidad o conseguir clasificaciones seguras, la eliminación o deshabilitación de los protocolos o conjuntos de cifrado más débiles se ha convertido en un.  En el resto de este documento se proporcionan instrucciones sobre cómo habilitar o deshabilitar determinados protocolos y conjuntos de cifrado.

Las claves del registro siguientes se encuentran en la misma ubicación: HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Use regedit o PowerShell para habilitar o deshabilitar estos protocolos y conjuntos de cifrado.

![Ubicación del Registro](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Habilitar y deshabilitar SSL 2,0
Use las siguientes claves del registro y sus valores para habilitar y deshabilitar SSL 2,0.

### <a name="enable-ssl-20"></a>Habilitar SSL 2,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server] "DisabledByDefault" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Client] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Client] "DisabledByDefault" = dword: 00000000 

### <a name="disable-ssl-20"></a>Deshabilitar SSL 2,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server] "DisabledByDefault" = dword: 00000001 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Client] "DisabledByDefault" = dword: 00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Uso de PowerShell para deshabilitar SSL 2,0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Habilitar y deshabilitar SSL 3,0
Use las siguientes claves del registro y sus valores para habilitar y deshabilitar SSL 3,0.

### <a name="enable-ssl-30"></a>Habilitar SSL 3.0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "DisabledByDefault" = dword: 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Client] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Client] "DisabledByDefault" = dword: 00000000 

### <a name="disable-ssl-30"></a>Deshabilitar SSL 3,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "DisabledByDefault" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Client] "DisabledByDefault" = dword: 00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Uso de PowerShell para deshabilitar SSL 3,0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>Habilitación y deshabilitación de TLS 1,0
Use las siguientes claves del registro y sus valores para habilitar y deshabilitar TLS 1,0.

> [!IMPORTANT]
> Al deshabilitar TLS 1,0 se interrumpirá el WAP para AD FS confianza.  Si deshabilita TLS 1,0, debe habilitar la autenticación segura para las aplicaciones.  Consulte [habilitación de la autenticación segura](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Habilitar TLS 1,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "DisabledByDefault" = dword: 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Client] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Client] "DisabledByDefault" = dword: 00000000 

### <a name="disable-tls-10"></a>Deshabilitar TLS 1,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "DisabledByDefault" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Client] "DisabledByDefault" = dword: 00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Uso de PowerShell para deshabilitar TLS 1,0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>Habilitación y deshabilitación de TLS 1,1
Use las siguientes claves del registro y sus valores para habilitar y deshabilitar TLS 1,1.

### <a name="enable-tls-11"></a>Habilitar TLS 1,1
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "DisabledByDefault" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Client] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Client] "DisabledByDefault" = dword: 00000000

### <a name="disable-tls-11"></a>Deshabilitar TLS 1,1
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "DisabledByDefault" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Client] "DisabledByDefault" = dword: 00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Uso de PowerShell para deshabilitar TLS 1,1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>Habilitación y deshabilitación de TLS 1,2

Use las siguientes claves del registro y sus valores para habilitar y deshabilitar TLS 1,2.

### <a name="enable-tls-12"></a>Habilitar TLS 1,2
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "DisabledByDefault" = dword: 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client] "DisabledByDefault" = dword: 00000000

### <a name="disable-tls-12"></a>Deshabilitar TLS 1,2
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "DisabledByDefault" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client] "DisabledByDefault" = dword: 00000001

### <a name="using-powershell-to-disable-tls-12"></a>Uso de PowerShell para deshabilitar TLS 1,2

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

Use las siguientes claves del registro y sus valores para habilitar y deshabilitar RC4.  Las claves del registro de este conjunto de cifrado se encuentran aquí:

- HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Ubicación del Registro](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Habilitar RC4

- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Habilitado" = dword: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Habilitado" = dword: 00000001 

### <a name="disable-rc4"></a>Deshabilitar RC4

- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = dword: 00000000 

### <a name="using-powershell"></a>Uso de PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Habilitar o deshabilitar conjuntos de cifrado adicionales

Puede deshabilitar determinados cifrados específicos si los quita de HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 

![Ubicación del Registro](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Para habilitar un conjunto de cifrado, agregue su valor de cadena a la clave de valor de cadena múltiple de funciones.  Por ejemplo, si queremos habilitar TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521, lo agregaremos a la cadena.

Para obtener una lista completa de los conjuntos de cifrado compatibles, consulte [conjuntos de cifrado en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).  En este documento se proporciona una tabla de conjuntos que están habilitados de forma predeterminada y los que se admiten, pero que no están habilitados de forma predeterminada.  Para priorizar los conjuntos de cifrado, consulte [priorización de conjuntos de cifrado Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Habilitación de la autenticación segura para aplicaciones .NET
Las aplicaciones .NET Framework 3.5/4.0/4.5. x pueden cambiar el protocolo predeterminado a TLS 1,2 habilitando la clave del registro SchUseStrongCrypto.  Esta clave del registro forzará a las aplicaciones .NET a usar TLS 1,2.

> [!IMPORTANT]
> Para AD FS en Windows Server 2016 y Windows Server 2012 R2, debe usar la clave .NET Framework 4.0/4.5. x: HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\\. NETFramework\v4.0.30319


En el .NET Framework 3,5, use la siguiente clave del registro:

[HKEY_LOCAL_MACHINE \SOFTWARE\Wow6432Node\Microsoft\\. NETFramework\v2.0.50727] "SchUseStrongCrypto" = dword: 00000001

Para el .NET Framework 4.0/4.5. x, use la siguiente clave del registro: HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\\. NETFramework\v4.0.30319 "SchUseStrongCrypto" = dword: 00000001

![Autenticación segura](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Información adicional

- [Conjuntos de cifrado en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)
- [Conjuntos de cifrado TLS en Windows 8.1](https://msdn.microsoft.com/library/windows/desktop/mt767781.aspx)
- [Dar prioridad a los conjuntos de cifrado Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)
- [Hablar de los cifrados y otros términos de Enigmatic](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
