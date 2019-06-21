---
title: Administración del control de acceso basado en roles con Windows PowerShell
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11dd417be4720b09851fc03f111edaaf06b55e59
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282134"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Administración del control de acceso basado en roles con Windows PowerShell

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para aprender a usar IPAM para administrar el control de acceso basado en roles con Windows PowerShell.  
  
>[!NOTE]
>Para la referencia de comandos de IPAM de Windows PowerShell, consulte el [IpamServer cmdlets de Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps).  
  
Los nuevos comandos de IPAM de Windows PowerShell proporcionan la capacidad de recuperar y cambiar los ámbitos de acceso de los objetos DNS y DHCP. En la tabla siguiente se muestra el comando correcto que se usará para cada objeto IPAM.  
  
|Objeto IPAM|Comando|Descripción|  
|---------------|-----------|---------------|  
|Servidor DNS|Get-IpamDnsServer|Este cmdlet devuelve el objeto de servidor DNS en IPAM|  
|Zona DNS|Get-IpamDnsZone|Este cmdlet devuelve el objeto de zona DNS en IPAM|  
|Registro de recursos DNS|Get-IpamResourceRecord|Este cmdlet devuelve el objeto de registro de recursos DNS en IPAM|  
|Reenviador condicional de DNS|Get-IpamDnsConditionalForwarder|Este cmdlet devuelve el objeto de reenviador condicional de DNS de IPAM|  
|Servidor DHCP|Get-IpamDhcpServer|Este cmdlet devuelve el objeto de servidor DHCP de IPAM|  
|Superámbito DHCP|Get-IpamDhcpSuperscope|Este cmdlet devuelve el objeto de superámbito DHCP en IPAM|  
|Ámbito DHCP|Get-IpamDhcpScope|Este cmdlet devuelve el objeto de ámbito DHCP de IPAM|  
  
En el siguiente ejemplo de salida del comando, el `Get-IpamDnsZone` cmdlet recupera la **dublin.contoso.com** zona DNS.  
  
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
Puede establecer ámbitos de acceso en objetos IPAM mediante el `Set-IpamAccessScope` comando. Puede usar este comando para establecer el ámbito de acceso a un valor específico para un objeto o para hacer que los objetos que se va a heredar ámbito de acceso de los objetos primarios. A continuación es los objetos que se pueden configurar con este comando.  
  
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
  
-   Subred de direcciones IP  
  
Siguiente es la sintaxis para el `Set-IpamAccessScope` comando.  
  
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
  
En el ejemplo siguiente, el ámbito de acceso de la zona DNS **dublin.contoso.com** se cambia de **Dublín** a **Europa**.  
  
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
  


