---
title: Administración del control de acceso basado en roles con Windows PowerShell
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 99859a1e9f43baa969475628e743975c78ef8bff
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866474"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Administración del control de acceso basado en roles con Windows PowerShell

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo usar IPAM para administrar el control de acceso basado en roles con Windows PowerShell.

>[!NOTE]
>Para ver la referencia de comandos de Windows PowerShell para IPAM, consulte los [cmdlets de IpamServer en Windows PowerShell](/powershell/module/ipamserver/).

Los nuevos comandos IPAM de Windows PowerShell le proporcionan la capacidad de recuperar y cambiar los ámbitos de acceso de los objetos DNS y DHCP. En la tabla siguiente se muestra el comando correcto que se debe usar para cada objeto IPAM.

|Objeto IPAM|Get-Help|Descripción|
|---------------|-----------|---------------|
|Servidor DNS|Get-IpamDnsServer|Este cmdlet devuelve el objeto de servidor DNS en IPAM|
|Zona DNS|Get-IpamDnsZone|Este cmdlet devuelve el objeto de zona DNS en IPAM|
|Registro de recursos DNS|Get-IpamResourceRecord|Este cmdlet devuelve el objeto de registro de recursos DNS en IPAM|
|Reenviador condicional de DNS|Get-IpamDnsConditionalForwarder|Este cmdlet devuelve el objeto de reenviador condicional de DNS en IPAM|
|Servidor DHCP|Get-IpamDhcpServer|Este cmdlet devuelve el objeto de servidor DHCP en IPAM|
|Superámbito DHCP|Get-IpamDhcpSuperscope|Este cmdlet devuelve el objeto de superámbito DHCP en IPAM|
|Ámbito DHCP|Get-IpamDhcpScope|Este cmdlet devuelve el objeto de ámbito DHCP en IPAM|

En el siguiente ejemplo de la salida del comando, el `Get-IpamDnsZone` cmdlet recupera la zona DNS **Dublin.contoso.com** .

```
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com

ZoneName             : dublin.contoso.com
ZoneType             : Forward
AccessScopePath      : \Global\Dublin
IsSigned             : False
DynamicUpdateStatus  : None
ScavengeStaleRecords : False
```

## <a name="setting-access-scopes-on-ipam-objects"></a>Establecer ámbitos de acceso en objetos IPAM
Puede establecer ámbitos de acceso en objetos IPAM mediante el `Set-IpamAccessScope` comando. Puede usar este comando para establecer el ámbito de acceso en un valor específico para un objeto o para que los objetos hereden el ámbito de acceso de los objetos primarios. A continuación se muestran los objetos que se pueden configurar con este comando.

-   Ámbito DHCP

-   Servidor DHCP

-   Superámbito DHCP

-   Reenviador condicional de DNS

-   Registros de recursos DNS

-   Servidor DNS

-   Zona DNS

-   IP Address Block

-   Intervalo de direcciones IP

-   Espacio de direcciones IP

-   Subred de dirección IP

A continuación se encuentra la sintaxis del `Set-IpamAccessScope` comando.

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

En el ejemplo siguiente, se cambia el ámbito de acceso de la zona DNS **Dublin.contoso.com** de **Dublín** a **Europa**.

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
