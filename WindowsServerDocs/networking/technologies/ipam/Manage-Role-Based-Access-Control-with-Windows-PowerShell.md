---
title: Administrar la función a partir del Control de acceso con Windows PowerShell
description: Este tema es parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: df6fa423a4ec891f1ad3faefad6c6054519542c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Administrar la función a partir del Control de acceso con Windows PowerShell

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo usar IPAM para administrar el control de acceso basado en roles con Windows PowerShell.  
  
>[!NOTE]
>Para la referencia de comandos de IPAM Windows PowerShell, consulta [Cmdlets del servidor de administración de direcciones IP (IPAM) en Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  
  
Los nuevos comandos de Windows PowerShell IPAM proporcionan la capacidad de recuperar y cambiar los ámbitos de acceso de objetos DNS y DHCP. La tabla siguiente muestra el comando que se usará para cada objeto IPAM correcto.  
  
|Objeto IPAM|Comando|Descripción|  
|---------------|-----------|---------------|  
|Servidor DNS|Get-IpamDnsServer|Este cmdlet devuelve el objeto de servidor DNS de IPAM|  
|Zona DNS|Get-IpamDnsZone|Este cmdlet devuelve el objeto de la zona DNS de IPAM|  
|Registro de recursos DNS|Get-IpamResourceRecord|Este cmdlet devuelve el objeto de registro de recursos DNS de IPAM|  
|Reenviador condicional de DNS|Get-IpamDnsConditionalForwarder|Este cmdlet devuelve el objeto de reenviador condicional de DNS de IPAM|  
|Servidor DHCP|Get-IpamDhcpServer|Este cmdlet devuelve el objeto de servidor DHCP de IPAM|  
|Ámbito superior DHCP|Get-IpamDhcpSuperscope|Este cmdlet devuelve el objeto de ámbito superior DHCP de IPAM|  
|Ámbito DHCP|Get-IpamDhcpScope|Este cmdlet devuelve el objeto de ámbito DHCP de IPAM|  
  
En el siguiente ejemplo de salida del comando, la `Get-IpamDnsZone` cmdlet recupera la **dublin.contoso.com** zona DNS.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>Definición de ámbitos de acceso en objetos IPAM  
Puedes establecer los ámbitos de acceso en objetos IPAM mediante el uso de la `Set-IpamAccessScope` comando. Puedes usar este comando para establecer el ámbito de acceso a un valor específico para un objeto o para hacer que los objetos que heredan el ámbito de acceso de objetos primarios. Siguiente es los objetos que se pueden configurar con este comando.  
  
-   Ámbito DHCP  
  
-   Servidor DHCP  
  
-   Ámbito superior DHCP  
  
-   Reenviador condicional de DNS  
  
-   Registros de recursos DNS  
  
-   Servidor DNS  
  
-   Zona DNS  
  
-   Bloque de direcciones IP  
  
-   Intervalo de direcciones IP  
  
-   Espacio de direcciones IP  
  
-   UNA subred de direcciones  
  
La siguiente es la sintaxis para la `Set-IpamAccessScope` comando.  
  
```  
NAME  
    Set-IpamAccessScope  
  
SYNTAX  
    Set-IpamAccessScope [-IpamRange] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpSuperscope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpScope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsConditionalForwarder] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsResourceRecord] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsZone] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamAddressSpace] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamSubnet] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamBlock] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
```  
  
En el siguiente ejemplo, el ámbito de acceso de la zona DNS **dublin.contoso.com** cambia de **Dublin** a **Europa**.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
  
PS C:\Users\Administrator.CONTOSO> $a = Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
PS C:\Users\Administrator.CONTOSO> Set-IpamAccessScope -IpamDnsZone -InputObject $a -AccessScopePath \Global\Europe -PassThru  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Europe  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  


