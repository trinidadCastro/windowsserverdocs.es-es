---
title: Desarrollo para Nano Server
description: Comunicación remota de PowerShell y sesiones CIM
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 57079470-a1c1-4fdc-af15-1950d3381860
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 6cf21d9db4221fd6bd76cfd5c362bb9f168d1ce9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360318"
---
# <a name="developing-for-nano-server"></a>Desarrollo para Nano Server

>Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Echa un vistazo a [Cambios en Nano Server](nano-in-semi-annual-channel.md) para más información. 

En estos temas se explican las diferencias importantes de PowerShell en Nano Server y también se proporcionan instrucciones para desarrollar sus propios cmdlets de PowerShell para usarlos en Nano Server.

- [PowerShell en Nano Server](PowerShell-on-Nano-Server.md)
- [Desarrollo de cmdlets de PowerShell para Nano Server](Developing-PowerShell-Cmdlets-for-Nano-Server.md)

## <a name="using-windows-powershell-remoting"></a>Uso de la comunicación remota a Windows PowerShell  
Para administrar Nano Server con la comunicación remota a Windows PowerShell, necesita agregar la dirección IP de Nano Server a la lista de hosts de confianza del equipo de administración, agregar la cuenta que utiliza para los administradores de Nano Server y habilitar CredSSP si piensa usar esta característica.  

> [!NOTE]
> Si el destino en Nano Server y el equipo de administración están en el mismo bosque de AD DS (o en bosques con una relación de confianza), no debe agregar Nano Server a la lista de hosts de confianza; puede conectarse a Nano Server mediante su nombre de dominio completo, por ejemplo: PS C:\> Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)
  
  
Para agregar Nano Server a la lista de hosts de confianza, ejecute este comando en un símbolo del sistema de Windows PowerShell con privilegios elevados:  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
Para iniciar la sesión remota de Windows PowerShell, inicie una sesión local de Windows PowerShell con privilegios elevados y, después, ejecute estos comandos:  
  
  
```  
$ip = "\<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
Ahora puede ejecutar comandos de Windows PowerShell en Nano Server de la forma habitual.  
  
> [!NOTE]  
> No todos los comandos de Windows PowerShell están disponibles en esta versión de Nano Server. Para ver cuáles están disponibles, ejecute `Get-Command -CommandType Cmdlet`.  
  
Detenga la sesión remota con el comando `Exit-PSSession`.  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>Uso de sesiones CIM en Windows PowerShell a través de WinRM  
Puede usar sesiones e instancias CIM en Windows PowerShell para ejecutar comandos WMI mediante la Administración remota de Windows (WinRM).  
  
Inicie la sesión CIM ejecutando estos comandos en un símbolo del sistema de Windows PowerShell:  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
Con la sesión establecida, puede ejecutar varios comandos WMI, por ejemplo:  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  