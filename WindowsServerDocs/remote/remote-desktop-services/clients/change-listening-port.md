---
title: Cambio del puerto de escucha en Escritorio remoto
description: Aprende a cambiar el puerto de escucha para el cliente de Escritorio remoto.
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2ace6e153b50ccf366359c70c63cf73676975ec
ms.sourcegitcommit: f86366371ed566526da211daee4e5c83eb6e37b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96843093"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Cambia el puerto de escucha para Escritorio remoto en el equipo

> Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2008 R2

Cuando te conectas a un equipo (ya sea un cliente de Windows o Windows Server) mediante el cliente de Escritorio remoto, la característica de Escritorio remoto del equipo "escucha" la solicitud de conexión a través de un puerto de escucha definido (el puerto 3389 de forma predeterminada). Puedes cambiar ese puerto de escucha en los equipos de Windows modificando el registro.

1. Inicia el Editor del Registro. (Escribe regedit en el cuadro de búsqueda).
2. Ve hasta la siguiente subclave del Registro: **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp**
3. Busque **PortNumber**.
4. Haz clic en **Editar > Modificar** y, a continuación, haz clic en **Decimal**.
5. Escribe el nuevo número de puerto y, a continuación, haz clic en **Aceptar**. 
6. Cierra el Editor del Registro y reinicia el equipo.

La próxima vez que te conectes a este equipo mediante el uso de Conexión a Escritorio remoto, deberás escribir el nuevo puerto. Si usas un firewall, asegúrate de configurarlo para que permita las conexiones al nuevo número de puerto.


Puede comprobar el puerto actual ejecutando el siguiente comando de PowerShell:

```powershell
Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "PortNumber"
```

Por ejemplo:

```powershell
PortNumber   : 3389
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations
PSChildName  : RDP-Tcp
PSDrive      : HKLM
PSProvider   : Microsoft.PowerShell.Core\Registry
```

Puede comprobar el puerto RDP ejecutando el siguiente comando de PowerShell. En este comando, especificaremos el nuevo puerto RDP como **3390**.


Para agregar un nuevo puerto RDP al registro:

```powershell
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "PortNumber" -Value 3390
New-NetFirewallRule -DisplayName 'RDPPORTLatest' -Profile 'Public' -Direction Inbound -Action Allow -Protocol TCP -LocalPort 3390
```
