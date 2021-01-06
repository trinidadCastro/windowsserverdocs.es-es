---
title: Referencia de comandos de Windows PowerShell de BGP
description: Puede usar este tema como referencia, al escribir scripts de Windows PowerShell en Windows Server 2016, para agregar, configurar y quitar las capacidades de BGP de los enrutadores de puerta de enlace de RAS y de red de área local (LAN) de acceso remoto.
manager: brianlic
ms.topic: article
ms.assetid: 4b0240a3-b927-4a1e-b241-5f8f29a9552f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c59b40e81f6bb5de5031d5b2ef259c97226a7209
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950091"
---
# <a name="bgp-windows-powershell-command-reference"></a>Referencia de comandos de Windows PowerShell de BGP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema como referencia, al escribir scripts de Windows PowerShell, para agregar, configurar y quitar las capacidades de BGP de los enrutadores de puerta de enlace de RAS y de red de área local (LAN) de acceso remoto.

Estos comandos BGP forman parte del conjunto de comandos de acceso remoto de Windows PowerShell para Windows Server 2016. Este tema le ayuda a encontrar rápidamente los comandos BGP que quiere usar en los scripts.

Para obtener más información sobre todos los comandos de acceso remoto, consulte [cmdlets de acceso remoto](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11)).

## <a name="bgp-command-reference"></a>Referencia de comandos de BGP
En las secciones siguientes se proporciona el nombre del comando, el propósito y la sintaxis de cada comando BGP, así como un vínculo al comando en la referencia de acceso remoto, que contiene información más detallada sobre cada comando.

Esta referencia contiene las siguientes secciones.

-   [Adición de comandos](#bkmk_add)

-   [Borrar comandos](#bkmk_clear)

-   [Deshabilitar y habilitar comandos](#bkmk_disable)

-   [Obtener comandos](#bkmk_get)

-   [Comandos de instalación](#bkmk_install)

-   [Quitar comandos](#bkmk_remove)

-   [Comandos SET](#bkmk_set)

-   [Comandos START y STOP](#bkmk_start)

-   [Comandos de desinstalación](#bkmk_uninstall)

### <a name="add-commands"></a><a name="bkmk_add"></a>Adición de comandos
A continuación se muestran los comandos de adición de BGP.

[Add-BgpCustomRoute](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Agrega rutas personalizadas a la tabla de enrutamiento de BGP.

```
Add-BgpCustomRoute [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-Interface <String[]> ] [-Network <String[]> ] [-PassThru] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Add-eliminara](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Agrega un nuevo par BGP.

```
Add-BgpPeer [-Name] <String> -LocalIPAddress <IPAddress> -PeerASN <UInt32> -PeerIPAddress <IPAddress> [-CimSession <CimSession[]> ] [-HoldTimeSec <UInt16> ] [-IdleHoldTimeSec <UInt16> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-LocalASN <UInt32> ] [-MaxAllowedPrefix <UInt32> ] [-OperationMode <OperationMode> {Mixed | Server} ] [-PassThru] [-PeeringMode <PeeringMode> {Automatic | Manual} ] [-RouteReflectorClient <Boolean> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Weight <UInt16> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Add-BgpRouteAggregate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Agrega una nueva ruta agregada para rutas BGP específicas.

```
Add-BgpRouteAggregate -Prefix <String> [-AttributePolicy <String[]> ] [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-PreserveASPath <PreserveASPath> ] [-RoutingDomain <String> ] [-SummaryOnly <SummaryOnly> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Add-BgpRouter](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Agrega un enrutador BGP para el ID. de inquilino especificado.

```
Add-BgpRouter -BgpIdentifier <IPAddress> -LocalASN <UInt32> [-CimSession <CimSession[]> ] [-ClientToClientReflection <ClientToClientReflection> ] [-ClusterId <UInt32> ] [-CompareMEDAcrossASN <Boolean> ] [-DefaultGatewayRouting <Boolean> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-IPv6Routing <IPv6RoutingState> {Disabled | Enabled} ] [-LocalIPv6Address <IPAddress> ] [-PassThru] [-RouteReflector <RouteReflector> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-TransitRouting <TransitRouting> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Add-BgpRoutingPolicy](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Agrega una directiva de enrutamiento de BGP al almacén de directivas.

```
Add-BgpRoutingPolicy [-Name] <String> [-PolicyType] <PolicyType> {Deny | Allow | ModifyAttribute} [-AddCommunity <String[]> ] [-CimSession <CimSession[]> ] [-ClearMED] [-Force] [-IgnorePrefix <String[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MatchASNRange <UInt32[]> ] [-MatchCommunity <String[]> ] [-MatchNextHop <IPAddress[]> ] [-MatchPrefix <String[]> ] [-NewLocalPref <UInt32]> ] [-NewMED <UInt32]> ] [-NewNextHop <IPAddress> ] [-PassThru] [-RemoveAllCommunities] [-RemoveCommunity <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Add-BgpRoutingPolicyForPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Agrega directivas de enrutamiento BGP a pares BGP.

```
Add-BgpRoutingPolicyForPeer -Direction <PolicyDirection> {Ingress | Egress} -PolicyName <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

### <a name="clear-commands"></a><a name="bkmk_clear"></a>Borrar comandos
A continuación se indican los comandos Clear para BGP

[Clear-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Borra la información de estabilización ligera de rutas para el conjunto especificado de rutas BGP.

```
Clear-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

### <a name="disable-and-enable-commands"></a><a name="bkmk_disable"></a>Deshabilitar y habilitar comandos
A continuación se indican los comandos de deshabilitar y habilitar para BGP

[Disable-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Deshabilita la amortiguación de rutas para las rutas BGP de oscilación.

```
Disable-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Enable-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Habilita la amortiguación de rutas para las rutas BGP de oscilación.

```
Enable-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

### <a name="get-commands"></a><a name="bkmk_get"></a>Obtener comandos
A continuación se muestran los comandos GET para BGP.

[Get-BgpCustomRoute](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Obtiene información de ruta personalizada del enrutador BGP.

```
Get-BgpCustomRoute [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Get-eliminara](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Obtiene información de configuración para los pares BGP.

```
Get-BgpPeer [[-Name] <String[]> ] [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Get-BgpRouteAggregate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Obtiene todas las rutas BGP agregadas configuradas por el administrador.

```
Get-BgpRouteAggregate [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Get-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Recupera la configuración de un motor de estabilización de rutas BGP.

```
Get-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Get-BgpRouteInformation](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Recupera información de la ruta BGP para uno o más prefijos de red de la tabla de enrutamiento BGP.

```
Get-BgpRouteInformation [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Network <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Type <RouteType> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Get-BgpRouter](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Obtiene información de configuración para los enrutadores BGP.

```
Get-BgpRouter [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String[]> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Get-BgpRoutingPolicy](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Obtiene la información de configuración de las directivas de enrutamiento BGP.

```
Get-BgpRoutingPolicy [[-Name] <String[]> ] [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PolicyType <PolicyType> {Deny | Allow | ModifyAttribute} ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Get-BgpStatistics](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Recupera estadísticas de anuncios de rutas y mensajes relacionados con el emparejamiento BGP.

```
Get-BgpStatistics [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

### <a name="install-commands"></a><a name="bkmk_install"></a>Comandos de instalación
A continuación se indican los comandos de instalación para la puerta de enlace RAS y BGP.

[Instalación-acceso remoto](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Realiza comprobaciones de requisitos previos de DirectAccess (DA) para asegurarse de que se puede instalar, instala DA para acceso remoto (RA) (incluye la administración de clientes remotos) o para la administración de clientes remotos solamente, instala VPN (VPN de acceso remoto y VPN de sitio a sitio) e instala el enrutamiento de BGP.

```
Parameter Set: MultiTenant
Install-RemoteAccess [-MultiTenancy] [-CapacityKbps <UInt64> ] [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MsgAuthenticator <String> {Enabled | Disabled} ] [-PassThru] [-RadiusPort <UInt16> ] [-RadiusScore <Byte> ] [-RadiusServer <String> ] [-RadiusTimeout <UInt32> ] [-SharedSecret <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]

Parameter Set: Vpn
Install-RemoteAccess [-VpnType] <String> {Vpn | VpnS2S | SstpProxy | RoutingOnly} [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-EntrypointName <String> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-IPAddressRange <String[]> ] [-IPv6Prefix <String> ] [-Legacy] [-MsgAuthenticator <String> {Enabled | Disabled} ] [-PassThru] [-RadiusPort <UInt16> ] [-RadiusScore <Byte> ] [-RadiusServer <String> ] [-RadiusTimeout <UInt32> ] [-SharedSecret <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

> [!IMPORTANT]
> Al instalar la puerta de enlace RAS en el modo multiinquilino, debe especificar si se habilita BGP para cada inquilino mediante el comando **enable-RemoteAccessRoutingDomain** de Windows PowerShell con el valor del parámetro **-Type** de **All**. En el ejemplo de código siguiente se muestra cómo instalar RAS en modo multiinquilino con todas las características de RAS (VPN de punto a sitio, VPN de sitio a sitio y enrutamiento BGP) habilitadas para dos inquilinos, contoso y fabrikam.

```
$Contoso_RoutingDomain = "ContosoTenant"
$Fabrikam_RoutingDomain = "FabrikamTenant"

Install-RemoteAccess -MultiTenancy

Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru
```

Si usa el acceso remoto como un enrutador LAN en lugar de una puerta de enlace, todavía puede usar BGP, que ofrece la ventaja de tener el enrutamiento dinámico en la intranet. Para instalar el acceso remoto como un enrutador LAN BGP, escriba el siguiente comando en un símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.

```
Install-RemoteAccess -VpnType RoutingOnly
```

### <a name="remove-commands"></a><a name="bkmk_remove"></a>Quitar comandos
A continuación se muestran los comandos de eliminación de BGP.

[Remove-BgpCustomRoute](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Quita las rutas personalizadas del enrutador BGP.

```
Remove-BgpCustomRoute [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-Interface <String[]> ] [-Network <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Remove-eliminara](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Quita los pares BGP de un enrutador.

```
Remove-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Remove-BgpRouteAggregate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Quita el conjunto de rutas BGP agregadas especificadas.

```
Remove-BgpRouteAggregate [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Remove-BgpRouter](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Quita un enrutador BGP.

```
Remove-BgpRouter [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String[]> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Remove-BgpRoutingPolicy](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Quita las directivas de enrutamiento del almacén de directivas.

```
Remove-BgpRoutingPolicy [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Remove-BgpRoutingPolicyForPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Quita las directivas de enrutamiento de los pares BGP.

```
Parameter Set: Remove1
Remove-BgpRoutingPolicyForPeer [-CimSession <CimSession[]> ] [-Direction <PolicyDirection> {Ingress | Egress} ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-PolicyName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

### <a name="set-commands"></a><a name="bkmk_set"></a>Comandos SET
A continuación se indican los comandos SET para BGP.

[Set-eliminara](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Actualiza la configuración del elemento BGP del mismo nivel especificado.

```
Set-BgpPeer [-Name] <String> [-CimSession <CimSession[]> ] [-ClearPrefixLimit] [-Force] [-HoldTimeSec <UInt16> ] [-IdleHoldTimeSec <UInt16> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-LocalASN <UInt32> ] [-LocalIPAddress <IPAddress> ] [-MaxAllowedPrefix <UInt32> ] [-OperationMode <OperationMode> {Mixed | Server} ] [-PassThru] [-PeerASN <UInt32> ] [-PeeringMode <PeeringMode> {Automatic | Manual} ] [-PeerIPAddress <IPAddress> ] [-RouteReflectorClient <Boolean> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Weight <UInt16> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Set-BgpRouteAggregate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Actualiza las propiedades de la ruta BGP agregada especificada.

```
Set-BgpRouteAggregate -Prefix <String> [-AttributePolicy <String[]> ] [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-PreserveASPath <PreserveASPath> ] [-RoutingDomain <String> ] [-SummaryOnly <SummaryOnly> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Set-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Configura el motor de estabilización de rutas BGP.

```
Set-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-HalfLife <UInt32> ] [-HalfLifeUnreachable <UInt32> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-MaxSuppressTime <UInt32> ] [-PassThru] [-ReuseThreshold <UInt32> ] [-RoutingDomain <String> ] [-SuppressThreshold <UInt32> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Set-BgpRouter](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Actualiza la configuración del enrutador BGP local para el ID. de inquilino especificado.

```
Set-BgpRouter [-BgpIdentifier <IPAddress> ] [-CimSession <CimSession[]> ] [-ClientToClientReflection <ClientToClientReflection> ] [-ClusterId <UInt32> ] [-CompareMEDAcrossASN <Boolean> ] [-DefaultGatewayRouting <Boolean> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-IPv6Routing <IPv6RoutingState> {Disabled | Enabled} ] [-LocalASN <UInt32> ] [-LocalIPv6Address <IPAddress> ] [-PassThru] [-RouteReflector <RouteReflector> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-TransitRouting <TransitRouting> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Set-BgpRoutingPolicy](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Modifica una configuración de directiva de enrutamiento.

```
Set-BgpRoutingPolicy [-Name] <String> [-AddCommunity <String[]> ] [-CimSession <CimSession[]> ] [-ClearMED] [-Force] [-IgnorePrefix <String[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MatchASNRange <UInt32[]> ] [-MatchCommunity <String[]> ] [-MatchNextHop <IPAddress[]> ] [-MatchPrefix <String[]> ] [-NewLocalPref <UInt32]> ] [-NewMED <UInt32]> ] [-NewNextHop <IPAddress> ] [-PassThru] [-PolicyType <PolicyType> {Deny | Allow | ModifyAttribute} ] [-RemoveAllCommunities] [-RemoveCommunity <String[]> ] [-RemovePolicyClause <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Set-BgpRoutingPolicyForPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Modifica las directivas de enrutamiento BGP para los pares BGP.

```
Set-BgpRoutingPolicyForPeer -Direction <PolicyDirection> {Ingress | Egress} -PolicyName <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

### <a name="start-and-stop-commands"></a><a name="bkmk_start"></a>Comandos START y STOP
A continuación se indican los comandos de inicio y detención para BGP.

[Start-eliminara](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Inicia las sesiones de enrutamiento para los pares BGP.

```
Start-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]
```

[Stop-eliminara](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Detiene las sesiones de enrutamiento para los pares BGP.

```
Stop-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

### <a name="uninstall-commands"></a><a name="bkmk_uninstall"></a>Comandos de desinstalación
A continuación se muestran los comandos de desinstalación para la puerta de enlace RAS y BGP.

[Desinstalar-RemoteAccess](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))

Desinstala el acceso remoto del equipo, incluidas todas las características y funcionalidades de acceso remoto (puerta de enlace RAS, BGP, etc.).

```
Uninstall-RemoteAccess [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-EntrypointName <String> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-ThrottleLimit <Int32> ] [-VpnType <String> {Vpn | VpnS2S} ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]
```

