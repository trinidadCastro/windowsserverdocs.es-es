---
title: PowerShell en Nano Server
description: Diferencias en el conjunto reducido de características de PowerShell en Nano Server
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.topic: article
ms.assetid: 9b25b939-1e2c-4bed-a8d3-2a8e8e46b53d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 4879ae58c24596d64d24b6bece54d4c35837f00f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826768"
---
# <a name="powershell-on-nano-server"></a>PowerShell en Nano Server

> Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consulta [Cambios en Nano Server](nano-in-semi-annual-channel.md) para más información.

## <a name="powershell-editions"></a>Ediciones de PowerShell

A partir de la versión 5.1, PowerShell está disponible en diferentes ediciones que denotan distintos conjuntos de características y compatibilidad de la plataforma.

- **Desktop Edition:** Se basa en .NET Framework y proporciona compatibilidad con scripts y módulos destinados a versiones de PowerShell que se ejecutan en las ediciones de superficie completa de Windows, como Server Core y el escritorio de Windows.
- **Core Edition:** Se basa en .NET Core y proporciona compatibilidad con scripts y módulos destinados a versiones de PowerShell que se ejecutan en las ediciones de superficie reducida de Windows, como Nano Server y Windows IoT.

La edición de ejecución de PowerShell se muestra en la propiedad PSEdition de $PSVersionTable.
```powershell
$PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.14300.1000
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
CLRVersion                     4.0.30319.42000
BuildVersion                   10.0.14300.1000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Los autores del módulo pueden declarar sus módulos para que sean compatibles con una o más ediciones de PowerShell mediante la clave de manifiesto de módulo CompatiblePSEditions. Esta clave solo se admite en PowerShell 5.1 o posterior.
```powershell
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1
$moduleInfo = Test-ModuleManifest -Path \TestModuleWithEdition.psd1
$moduleInfo.CompatiblePSEditions
Desktop
Core

$moduleInfo | Get-Member CompatiblePSEditions

   TypeName: System.Management.Automation.PSModuleInfo

Name                 MemberType Definition
----                 ---------- ----------
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}

```
Al obtener una lista de los módulos disponibles, puede filtrar la lista de edición de PowerShell.
```powershell
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Desktop

    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0        ModuleWithPSEditions

Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Core | % CompatiblePSEditions
Desktop
Core

```
Los autores de scripts pueden impedir la ejecución de un script, a menos que se ejecute en una edición compatible de PowerShell mediante el parámetro PSEdition con una instrucción #requires.
```powershell
Set-Content C:\script.ps1 -Value #requires -PSEdition Core
Get-Process -Name PowerShell
Get-Content C:\script.ps1
#requires -PSEdition Core
Get-Process -Name PowerShell

C:\script.ps1
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a #requires statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.
At line:1 char:1
+ C:\script.ps1
+ ~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition
```

## <a name="differences-in-powershell-on-nano-server"></a>Diferencias de PowerShell en Nano Server
Nano Server incluye PowerShell Core de forma predeterminada en todas las instalaciones de Nano Server. PowerShell Core es una edición de superficie reducida de PowerShell que se basa en .NET Core y se ejecuta en las ediciones de superficie reducida de Windows, como Nano Server y Windows IoT Core. PowerShell Core funciona de la misma manera que otras ediciones de PowerShell, como Windows PowerShell se ejecutan en Windows Server 2016. Sin embargo, la superficie reducida de Nano Server significa que no todas las características de PowerShell de Windows Server 2016 están disponibles en PowerShell Core en Nano Server.


**Características de Windows PowerShell no disponibles en Nano Server**
* Adaptadores de tipo ADSI, ADO y WMI
* Enable-PSRemoting, Disable-PSRemoting (la comunicación remota de PowerShell está habilitada de forma predeterminada; vea la sección Uso del acceso remoto a Windows PowerShell de [Instalación de Nano Server](Getting-Started-with-Nano-Server.md)).
* Las tareas programadas y el módulo PSScheduledJob
* Cmdlets de equipo para unirse a un dominio { Add | Remove } (para ver métodos diferentes para unir Nano Server a un dominio, vea la sección Unión de Nano Server a un dominio de [Instalación de Nano Server](Getting-Started-with-Nano-Server.md)).
* Reset-ComputerMachinePassword, Test-ComputerSecureChannel
* Perfiles (puede agregar un script de inicio para las conexiones remotas entrantes con `Set-PSSessionConfiguration`)
* Cmdlets Clipboard
* Cmdlets EventLog { Clear | Get | Limit | New | Remove | Show | Write } (usar los cmdlets New-WinEvent y Get-WinEvent en su lugar)
* Cmdlet Get-PfxCertificate
* Cmdlets TraceSource { Get | Set }
* Cmdlets de contador { Get | Export | Import }
* Algunos cmdlets relacionados con web { New-WebServiceProxy, Send-MailMessage, ConvertTo-Html }
* Registro y seguimiento mediante el módulo PSDiagnostics
* Get-HotFix (para obtener y administrar las actualizaciones en Nano Server, vea [Administración de Nano Server](Manage-Nano-Server.md))
* Cmdlets de comunicación remota implícita { Export-PSSession | Import-PSSession }
* New-PSTransportOption
* Cmdlets Transaction y transacciones de PowerShell { Complete | Get | Start | Undo | Use }
* Cmdlets, módulos e infraestructura de flujo de trabajo de PowerShell
* Out-Printer
* Update-List
* Cmdlets de WMI v1: Get-WmiObject, Invoke-WmiMethod, Register-WmiEvent, Remove-WmiObject, Set-WmiInstance (usa el módulo de CimCmdlets en su lugar).

## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>Uso de la configuración de estado deseado de Windows PowerShell con Nano Server

Puede administrar Nano Server como nodos de destino con la configuración de estado deseado (DSC) de Windows PowerShell. Actualmente, puede administrar los nodos que ejecutan Nano Server con DSC en modo de inserción solo. No todas las características de DSC funcionan con Nano Server.

Para obtener todos los detalles, vea [Uso de DSC en Nano Server](https://docs.microsoft.com/powershell/scripting/dsc/getting-started/nanodsc).

