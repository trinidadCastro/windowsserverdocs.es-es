---
title: Personalizar el título de RDS "Recursos de trabajo" con PowerShell en Windows Server
description: Proporciona una descripción para cambiar el nombre del área de trabajo a uno distinto del predeterminado en Windows Server.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 43837826a6cddc2c3c4c7c1af874334718a3a067
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743340"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Personalizar el título de RDS "Recursos de trabajo" con PowerShell en Windows Server

Cuando se usa Windows Server para acceder a RemoteApp o a escritorios a través de Acceso web de Escritorio remoto o la nueva aplicación de Escritorio remoto, es posible que hayas visto que el área de trabajo se titula "Recursos de trabajo" de manera predeterminada.  Puedes cambiar fácilmente el título con cmdlets de PowerShell.

Para cambiar el título, abre una nueva ventana de PowerShell en el servidor del Agente de conexión e importa el módulo RemoteDesktop con el siguiente comando.

```powershell
    Import-Module RemoteDesktop
```

A continuación, usa el comando Set-RDWorkspace para cambiar el nombre del área de trabajo.

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

Por ejemplo, puedes usar el comando siguiente para cambiar el nombre del área de trabajo a "Contoso RemoteApps":

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

Si ejecutas a varios Agentes de conexión en modo de Alta disponibilidad, debes ejecutarlo en la instancia del agente activa. Puedes usar este comando:

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Para más información sobre el cmdlet Set-RDWorkspace, consulta la referencia [Set-RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps).