---
title: Delegación AD FS acceso Commandlet de PowerShell a usuarios que no son administradores
description: Este documento descirbes cómo delegar permisos a usuarios que no son administradores para AD FS PowerShell cmdlts.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 86bbb562e223fdf61dac3ce5646d97a57b2eba4c
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546306"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>Delegación AD FS acceso Commandlet de PowerShell a usuarios que no son administradores 
De forma predeterminada, la administración de AD FS a través de PowerShell solo la pueden realizar los administradores de AD FS. En el caso de muchas organizaciones de gran tamaño, es posible que no sea un modelo operativo viable al tratar con otros usuarios, como el personal del Departamento de soporte técnico.  

Con la suficiente administración (JEA), los clientes ahora pueden delegar commandlets específicos a distintos grupos de personal.  
Un buen ejemplo de este caso de uso es permitir al personal del Departamento de soporte técnico consultar AD FS estado de bloqueo de cuenta y restablecer el estado de bloqueo de cuenta en AD FS una vez que se ha probado un usuario. En este caso, el commandlets que se tendría que delegar es: 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

Usamos este ejemplo en el resto de este documento. Sin embargo, se puede personalizar esto para permitir la delegación a fin de establecer las propiedades de los usuarios de confianza y entregarlos a los propietarios de la aplicación dentro de la organización.  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>Crear los grupos requeridos necesarios para conceder permisos a los usuarios 
1. Cree una [cuenta de servicio administrada de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). La cuenta gMSA se usa para permitir que el usuario JEA tenga acceso a los recursos de red como otras máquinas o servicios Web. Proporciona una identidad de dominio que se puede usar para autenticarse en los recursos de cualquier equipo del dominio. A la cuenta gMSA se le conceden los derechos administrativos necesarios más adelante en la instalación. En este ejemplo, se llama a la cuenta **gMSAContoso**. 
2. Crear un grupo de Active Directory se puede rellenar con usuarios a los que es necesario conceder los derechos de los comandos delegados. En este ejemplo, al personal del Departamento de soporte técnico se le conceden permisos para leer, actualizar y restablecer el estado de bloqueo de ADFS. Nos referimos a este grupo a lo largo del ejemplo como **JEAContoso**. 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>Instale la cuenta gMSA en el servidor de ADFS: 
Cree una cuenta de servicio que tenga derechos administrativos en los servidores de ADFS. Esto puede realizarse en el controlador de dominio o de forma remota siempre que se instale el paquete de RSAT de AD.  La cuenta de servicio debe crearse en el mismo bosque que el servidor de ADFS. Modifique los valores de ejemplo para la configuración de la granja de servidores. 

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

Instale la cuenta gMSA en el servidor de ADFS.  Esto debe ejecutarse en todos los nodos de ADFS de la granja. 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>Conceder derechos de administrador de cuenta de gMSA 
Si la granja usa la administración delegada, conceda los derechos de administrador de la cuenta de gMSA agregándolo al grupo existente que tiene acceso de administrador delegado.  
 
Si la granja no utiliza la administración delegada, conceda los derechos de administrador de la cuenta de gMSA convirtiéndola en el grupo de administración local en todos los servidores de ADFS. 
 
 
### <a name="create-the-jea-role-file"></a>Crear el archivo de rol JEA 
 
En el servidor de AD FS, cree el rol JEA en un archivo del Bloc de notas. Las instrucciones para crear el rol se proporcionan en las [funcionalidades de rol de jea](https://docs.microsoft.com/powershell/jea/role-capabilities). 
 
El commandlets delegado en este ejemplo es `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`. 

Rol JEA de ejemplo que delega el acceso de "RESET-ADFSAccountLockout", "Get-ADFSAccountActivity" y "Set-ADFSAccountActivity" commandlets:

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>Crear el archivo de configuración de sesión de JEA 
Siga las instrucciones para crear el archivo de [configuración de sesión jea](https://docs.microsoft.com/powershell/jea/session-configurations) . El archivo de configuración determina quién puede usar el punto de conexión JEA y las funciones a las que tienen acceso. 

Se hace referencia a las funciones de rol mediante el nombre plano (nombre de archivo sin la extensión) del archivo de funcionalidad de rol. Si hay varias funcionalidades de rol disponibles en el sistema con el mismo nombre plano, PowerShell usa su orden de búsqueda implícito para seleccionar el archivo de funcionalidad de rol efectivo. No proporciona acceso a todos los archivos de funcionalidad de rol con el mismo nombre. 

Para especificar un archivo de funcionalidad de rol con una ruta de `RoleCapabilityFiles` acceso, use el argumento. En el caso de una subcarpeta, jea busca módulos de PowerShell válidos `RoleCapabilities` que contengan una `RoleCapabilityFiles` subcarpeta, donde el argumento debe `RoleCapabilities`modificarse para que sea. 

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
 
Se recomienda encarecidamente [probar el archivo de configuración de sesión](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1) si ha editado manualmente el archivo PSSC mediante un editor de texto para asegurarse de que la sintaxis es correcta. Si un archivo de configuración de sesión no pasa esta prueba, no se registra correctamente en el sistema.  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>Instalación de la configuración de sesión de JEA en el servidor de AD FS 

Instalación de la configuración de sesión de JEA en el servidor de AD FS 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>Instrucciones operativas 
Una vez configurado, se puede usar el registro y la auditoría de JEA para determinar si los usuarios correctos tienen acceso al punto de conexión de JEA. 

Para usar los comandos delegados: 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 


```
