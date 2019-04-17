---
title: "Administración de conjuntos de cifrado y protocolos SSL/TLS de AD FS"
description: "Preguntas más frecuentes de AD FS de 2016"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 23055a1b727e4f1a6b1ccafdea9a410c6021c432
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2018
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Administración de conjuntos de cifrado y protocolos SSL/TLS de AD FS
La siguiente documentación proporciona información acerca de cómo deshabilitar y habilitar ciertos protocolos TLS/SSL y conjuntos de cifrados que se usan por AD FS

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel y conjuntos de cifrado en AD FS

La seguridad de la capa de transporte (TLS) y la capa de Sockets seguros (SSL) son protocolos que proporcionan para proteger las comunicaciones.  Los servicios de federación de Active Directory usa estos protocolos para las comunicaciones.  Hoy en día existen varias versiones de estos protocolos.

Schannel es un proveedor de soporte técnico de seguridad (SSP) que implementa los protocolos de autenticación estándar SSL, TLS y DTLS Internet. La interfaz de proveedor de soporte técnico de seguridad (SSPI) es una API usada por los sistemas Windows para realizar funciones relacionadas con la seguridad, incluida la autenticación. La SSPI funciona como una interfaz común a varios proveedores de compatibilidad para seguridad (SSP), incluida la SSP. Schannel

Un conjunto de cifrado es un conjunto de algoritmos de cifrado. La implementación de SSP schannel de los protocolos TLS/SSL usan algoritmos de un conjunto de cifrado para crear claves y cifrar la información. Un conjunto de cifrado especifica un algoritmo para cada una de las siguientes tareas:

- Intercambio de claves
- Cifrado de forma masiva
- Autenticación de mensajes

AD FS utiliza Schannel.dll para realizar sus interacciones comunicaciones seguras.  Actualmente, AD FS admite todos los protocolos y conjuntos de cifrado que son compatibles con Schannel.dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Administración de los protocolos TLS/SSL y conjuntos de cifrado
> [!IMPORTANT]
> Esta sección contiene pasos que te indicamos cómo modificar el registro. Sin embargo, pueden producirse problemas graves si se modifica el registro de forma incorrecta. Por lo tanto, asegúrate de que sigue estos pasos cuidadosamente. 
> 
> Ten en cuenta que cambiar la configuración de seguridad predeterminada para SCHANNEL podría break o impedir la comunicación entre determinados clientes y servidores.  Esto ocurrirá si es necesaria la comunicación segura y no tienen un protocolo negociar comunicaciones con.
> 
> Si va a aplicar estos cambios, debe aplicarse a todos los servidores de AD FS en su conjunto.  Después de aplicar estos cambios se requiere un reinicio.

En el día de hoy y edad, reforzar los servidores y quitar anteriores o conjuntos de cifrado no segura se está convirtiendo en una prioridad más importante para muchas organizaciones.  Están disponibles los paquetes de software que se probar los servidores y proporcionar información detallada sobre estos protocolos y conjuntos de aplicaciones.  Para poder sigan siendo compatibles o lograr clasificaciones seguros, quitar o deshabilitar protocolos más débiles o conjuntos de cifrado se ha convertido en debe tener un.  El resto de este documento proporciona instrucciones sobre cómo habilitar o deshabilitar determinados protocolos y conjuntos de cifrado.

Las claves del registro siguientes se encuentran en la misma ubicación: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Usar regedit o PowerShell para habilitar o deshabilitar estos protocolos y conjuntos de cifrado.

![Ubicación del registro](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Habilitar y deshabilitar SSL 2.0
Usa las siguientes claves del registro y sus valores para habilitar y deshabilitar SSL 2.0.

### <a name="enable-ssl-20"></a>Habilitar SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault" = dword: 00000000 

### <a name="disable-ssl-20"></a>Deshabilitar SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault" = dword: 00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault" = dword: 00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Usar PowerShell para deshabilitar SSL 2.0

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
Usa las siguientes claves del registro y sus valores para habilitar y deshabilitar SSL 3.0.

### <a name="enable-ssl-30"></a>Habilitar SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault" = dword: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault" = dword: 00000000 

### <a name="disable-ssl-30"></a>Deshabilitar SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault" = dword: 00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Usar PowerShell para deshabilitar SSL 3.0

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
Usa las siguientes claves del registro y sus valores para habilitar y deshabilitar TLS 1.0.

> [!IMPORTANT]
> Deshabilitar TLS 1.0 interrumpirá el WAP a la confianza de AD FS.  Si deshabilitas TLS 1.0 debe habilitar la autenticación sólida para sus aplicaciones.  Consulta [habilitar la autenticación firme](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Habilitar TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault" = dword: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault" = dword: 00000000 

### <a name="disable-tls-10"></a>Deshabilitar TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault" = dword: 00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Usar PowerShell para deshabilitar TLS 1.0

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
Usa las siguientes claves del registro y sus valores para habilitar y deshabilitar TLS 1.1.

### <a name="enable-tls-11"></a>Habilitar TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault" = dword: 00000000

### <a name="disable-tls-11"></a>Deshabilitar TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault" = dword: 00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Usar PowerShell para deshabilitar TLS 1.1

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

Para habilitar y deshabilitar TLS 1.2, usa las siguientes claves del registro y sus valores.

### <a name="enable-tls-12"></a>Habilitar TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault" = dword: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault" = dword: 00000000

### <a name="disable-tls-12"></a>Deshabilitar TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault" = dword: 00000001

### <a name="using-powershell-to-disable-tls-12"></a>Usar PowerShell para deshabilitar TLS 1.2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>Habilitar y deshabilitar el cifrado RC4 

Para habilitar y deshabilitar el cifrado RC4, usa las siguientes claves del registro y sus valores.  Claves de registro de este conjunto de cifrado se encuentran aquí:

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Ubicación del registro](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Habilitar el cifrado RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = dword: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = dword: 00000001 

### <a name="disable-rc4"></a>Deshabilitar el cifrado RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = dword: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = dword: 00000000 

### <a name="using-powershell"></a>Uso de PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Habilitar o deshabilitar los conjuntos de cifrado adicionales

Puedes deshabilitar determinados cifrados específicos quitándolos de HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\0010002 

![Ubicación del registro](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Para habilitar un conjunto de cifrado, que es el valor de cadena a la clave de valor de cadena de varias funciones.  Por ejemplo, si queremos habilitar TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521, a continuación, nos agregarlo a la cadena.

Para obtener una lista completa de cifrado compatibles consulta conjuntos [conjuntos de cifrado de TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx).  Este documento proporciona una tabla de conjuntos de aplicaciones que están habilitadas de manera predeterminada y los que se admiten, pero no está habilitado de manera predeterminada.  Dar prioridad a los conjuntos de cifrado consulta [dar prioridad a conjuntos de cifrado Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Habilitar la autenticación seguras para las aplicaciones de .NET
Las aplicaciones de .NET Framework 3.5/4.0/4.5.x para cambiar el protocolo predeterminado TLS 1.2, habilitar la clave del registro SchUseStrongCrypto.  Esta clave del registro forzará a aplicaciones de .NET usar TLS 1.2.

> [!IMPORTANT]
> Para AD FS en Windows Server 2016 y Windows Server 2012 R2 debe usar la clave de .NET Framework 4.0 o 4.5: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\. NETFramework\v4.0.30319


Para .NET Framework 3.5 usa la siguiente clave del registro:

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\. NETFramework\v2.0.50727] "SchUseStrongCrypto" = dword: 00000001

Para .NET Framework 4.0 o 4.5x usa la siguiente clave del registro: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\. NETFramework\v4.0.30319 "SchUseStrongCrypto" = dword: 00000001

![Autenticación firme](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Información adicional

- [Conjuntos de cifrado de TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)
- [Conjuntos de cifrado TLS en Windows 8.1](https://msdn.microsoft.com/en-us/library/windows/desktop/mt767781.aspx)
- [Establecer prioridades de conjuntos de cifrado Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)
- [Habla con cifrados y otras lenguas Enigmatic](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
