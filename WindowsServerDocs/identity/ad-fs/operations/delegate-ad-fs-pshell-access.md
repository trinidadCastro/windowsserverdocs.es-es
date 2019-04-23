---
title: Delegar el acceso de Commandlet de Powershell de AD FS a los usuarios que no son administradores
description: Este documento descirbes cómo delegar permisos al grupo de administradores para cmdlets de PowerShell de AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 265d22b045011e34192e56bdb970955b601cda56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883566"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>Delegar el acceso de Commandlet de Powershell de AD FS a los usuarios que no son administradores 
De forma predeterminada, la administración de AD FS a través de PowerShell solo puede realizarse por los administradores de AD FS. Para muchas organizaciones grandes, esto no puede ser un modelo operativo viable cuando se trabaja con otras personas, como un personal de asistencia.  

Con Just Enough Administration (JEA), los clientes ahora pueden delegar los cmdlets específicos a grupos de personas diferentes.  
Un buen ejemplo de este caso de uso permite al personal de asistencia para consultar AD FS, el estado de bloqueo de la cuenta y restablece estado de bloqueo de cuenta de AD FS, una vez que un usuario se ha probado. En este caso, los cmdlets que deberán delegar son: 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

En este ejemplo se usa en el resto de este documento. Sin embargo, uno puede personalizarlo para también permitir la delegación establecer las propiedades de usuarios de confianza y pasar esta acción a los propietarios de aplicaciones dentro de la organización.  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>Crear los grupos necesarios debe conceder permisos a los usuarios 
1. Crear un [cuenta de servicio administrada de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). La cuenta de gMSA se usa para permitir que el usuario JEA tener acceso a los recursos de red como otros equipos o servicios web. Proporciona una identidad de dominio que se puede usar para autenticarse en recursos en cualquier equipo del dominio. La cuenta de gMSA se concede los derechos administrativos necesarios más adelante en el programa de instalación. En este ejemplo, llamamos a la cuenta **gMSAContoso**. 
2. Creación de un Active Directory grupo puede rellenarse con usuarios que necesitan que se deben conceder los derechos a los comandos delegados. En este ejemplo, al personal de asistencia se concede permisos para leer, actualizar y restablecer el estado de bloqueo de AD FS. Nos referimos a este grupo en todo el ejemplo como **JEAContoso**. 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>Instalar la cuenta de gMSA en el servidor de ADFS: 
Crear una cuenta de servicio que tenga derechos administrativos a los servidores ADFS. Esto puede realizarse en el controlador de dominio o remota siempre que esté instalado el paquete de RSAT de AD.  La cuenta de servicio debe crearse en el mismo bosque que el servidor de ADFS. Modifique los valores de ejemplo para la configuración de la granja de servidores. 

```powershell
 # This command should only be run if this is the first time gMSA accounts are enabled in the forest 
Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))  
 
# Run this on every node that you want to have JEA configured on  
$adfsServer = Get-ADComputer server01.contoso.com  
 
# Run targeted at domain controller  
$serviceaccount = New-ADServiceAccount gMSAcontoso -DNSHostName <FQDN of the domain containing the KDS key> - PrincipalsAllowedToRetrieveManagedPassword $adfsServer –passthru 
 
# Run this on every node 
Add-ADComputerServiceAccount -Identity server01.contoso.com -ServiceAccount $ServiceAccount 
```

Instale la cuenta de gMSA en el servidor de ADFS.  Esto debe ejecutarse en cada nodo ADFS de la granja de servidores. 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>Conceder la derechos de administrador de cuenta de gMSA 
Si la granja de servidores es utilizar la administración delegada, conceda a la gMSA derechos de administrador de cuenta, éste se agrega al grupo existente que haya delegado el acceso de administrador.  
 
Si la granja de servidores no está usando la administración delegada, conceda a la gMSA derechos de administrador de cuenta por lo que el grupo de administración local en todos los servidores ADFS. 
 
 
### <a name="create-the-jea-role-file"></a>Crear el archivo de rol JEA 
 
Crear el rol JEA en un archivo de Bloc de notas. Se proporcionan instrucciones para crear el rol en [las funcionalidades de rol JEA](https://docs.microsoft.com/powershell/jea/role-capabilities). 
 
Los commandlets de delegado en este ejemplo son `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`. 

Delegar el acceso de los cmdlets de 'Reset-ADFSAccountLockout', 'Get-ADFSAccountActivity' y 'Set-ADFSAccountActivity' el rol JEA de ejemplo:

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>Crear el archivo de configuración de sesión JEA 
Siga las instrucciones para crear el [configuración de sesión JEA](https://docs.microsoft.com/powershell/jea/session-configurations) archivo. El archivo de configuración determina quién puede usar el punto de conexión JEA y qué funcionalidades que tienen acceso. 

El nombre sin formato (nombre de archivo sin la extensión) del archivo de funcionalidad de rol hace referencia a las funcionalidades de rol. Si hay varias funcionalidades de rol en el sistema con el mismo nombre sin formato, PowerShell usa su orden de búsqueda implícito para seleccionar el archivo de funcionalidad de rol efectivo. No otorga acceso a todos los archivos de funcionalidad de rol con el mismo nombre. 

Para especificar un archivo de funcionalidad de rol con una ruta de acceso, use el `RoleCapabilityFiles` argumento. Para una subcarpeta, JEA buscará módulos válidos de Powershell que contienen un `RoleCapabilities` subcarpeta, donde el `RoleCapabilityFiles` argumento debe modificarse para que sea `RoleCapabilities`. 

Archivo de configuración de sesión de ejemplo: 

```powershell
@{
SchemaVersion = '2.0.0.0'
GUID = '54c8d41b-6425-46ac-a2eb-8c0184d9c6e6'
SessionType = 'RestrictedRemoteServer'
GroupManagedServiceAccount =  'CONTOSO\gMSAcontoso'
RoleDefinitions = @{ JEAcontoso = @{ RoleCapabilityFiles = 'C:\Program Files\WindowsPowershell\Modules\AccountActivityJEA\RoleCapabilities\JEAAccountActivityResetRole.psrc' } }
}
```

Guarde el archivo de configuración de sesión. 
 
Se recomienda encarecidamente a [probar su archivo de configuración de sesión](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1) si ha editado el archivo pssc manualmente con un editor de texto para asegurarse de que la sintaxis es correcta. Si un archivo de configuración de sesión no supera esta prueba, no está registrado correctamente en el sistema.  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>Instalar la configuración de sesión JEA en el servidor de AD FS 

Instalar la configuración de sesión JEA en el servidor de AD FS 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>Instrucciones de funcionamiento 
Una vez configurado, se puede usar JEA de registro y auditoría para saber si los usuarios correctos tengan acceso al punto de conexión JEA. 

Para usar los comandos delegados: 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 
```
