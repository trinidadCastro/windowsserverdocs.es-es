---
title: Personalizar el título de RDS "Recursos de trabajo" con PowerShell en Windows Server
description: Proporciona la descripción de cómo cambiar el nombre de área de trabajo de forma predeterminada en Windows Server.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 43837826a6cddc2c3c4c7c1af874334718a3a067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826716"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Personalizar el título de RDS "Recursos de trabajo" con PowerShell en Windows Server

Cuando se usa Windows Server para tener acceso a RemoteApp o escritorios a través de acceso Web de escritorio remoto o la nueva aplicación de escritorio remoto, es posible que haya observado que el área de trabajo se titula "Recursos de trabajo" de forma predeterminada.  Puede cambiar fácilmente el título mediante cmdlets de PowerShell.

Para cambiar el título, abra una nueva ventana de PowerShell en el servidor de agente de conexión e importe el módulo de RemoteDesktop con el siguiente comando.

```powershell
    Import-Module RemoteDesktop
```

A continuación, use el comando Set-RDWorkspace para cambiar el nombre del área de trabajo.

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

Por ejemplo, puede usar el comando siguiente para cambiar el nombre del área de trabajo a "Contoso RemoteApps":

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

Si ejecuta a varios agentes de conexión en modo de alta disponibilidad, debe ejecutar esto en la instancia de broker activa. Puede usar este comando:

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Para obtener más información sobre el cmdlet Set-RDWorkspace, consulte el [conjunto RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps) referencia.