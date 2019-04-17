---
title: Agregar una puerta de enlace Virtual a una red Virtual del inquilino
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9552054-4eb9-48db-a6ce-f36ae55addcd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 351cafd1466c4b3c20e907ea221c9f58129b3443
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-virtual-gateway-to-a-tenant-virtual-network"></a>Agregar una puerta de enlace Virtual a una red Virtual del inquilino

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo configurar el inquilino Virtual puertas de enlace, el uso de scripts y los cmdlets de Windows PowerShell para proporcionar de redes sus los de inquilinos virtuales con la conectividad de sitio a sitio a sus sitios de la organización y a Internet.   
  
Puerta de enlace de RAS admite a hasta cien inquilinos, según el ancho de banda que usa cada inquilino. Usa el controlador de red Virtual puertas de enlace a instancias de puerta de enlace de RAS que son miembros de grupos de puerta de enlace de inquilino de agregar. Controlador de red determina automáticamente la puerta de enlace RAS recomendados para usar al implementar una puerta de enlace Virtual nuevo para sus inquilinos.  
  
Cada puerta de enlace Virtual corresponde a un inquilino particular y consiste en uno o más conexiones de red (sitio a sitio los túneles VPN) y, opcionalmente, conexiones del protocolo de puerta de enlace de borde (BGP). Esto permite que los clientes puedan conectarse a su red Virtual del inquilino a una red externa, como un inquilino de red de empresa, una red de proveedor de servicio o a internet.  
  
Al implementar una puerta de enlace de inquilino Virtual, tienes las siguientes opciones de configuración:  
  
**Opciones de conexión de red**  
- Red privada virtual (VPN) de IPSec sitio a sitio   
- Encapsulación de enrutamiento genérico (GRE)   
- Reenvío de capa 3  
  
**Opciones de configuración BGP**  
- Configuración del enrutador BGP  
- Configuración del sistema del mismo nivel BGP  
- Configuración de directivas de enrutamiento de BGP  
  
Los scripts de ejemplo de Windows PowerShell y comandos en este tema muestran cómo implementar una puerta de enlace de inquilino virtual en una puerta de enlace de RAS con cada una de estas opciones.  
  
Este tema contiene las siguientes secciones.  
  
- [Agregar una puerta de enlace virtual para un inquilino](#bkmk_addgwy)  
- [Agregar una conexión de red VPN de sitio a sitio para un inquilino (IPsec, GRE o L3)](#bkmk_s2s1)  
- [Configurar la puerta de enlace como enrutador BGP](#bkmk_bgp1)  
- [Configurar una puerta de enlace con todos los tipos de conexión tres (IPsec, GRE, L3) y BGP](#bkmk_all3)  
- [Modificar o quitar una puerta de enlace para una red Virtual](#bkmk_modify)  
  
   
>[!IMPORTANT]  
>Antes de ejecutar cualquiera de los comandos de Windows PowerShell de ejemplo y los scripts que se proporcionan en este tema, debes cambiar todos los valores de variables para que los valores son adecuados para la implementación.  
  
## <a name="bkmk_addgwy"></a>Agregar una puerta de enlace virtual para un inquilino  
  
Paso 1: Comprobar que el objeto de grupo de puerta de enlace existe en el controlador de red.  
```  
$uri = "https://ncrest.contoso.com"   
  
# Retrieve the Gateway Pool configuration  
$gwPool = Get-NetworkControllerGatewayPool -ConnectionUri $uri  
  
# Display in JSON format  
$gwPool | ConvertTo-Json -Depth 2   
  
  
```  
Paso 2: Comprobar la existencia de la subred que se usará para el enrutamiento de paquetes fuera de red del inquilino Virtual en el controlador de red; y recupera la subred virtual que se utiliza para el enrutamiento entre la puerta de enlace de inquilino y red virtual.  
  
```  
$uri = "https://ncrest.contoso.com"   
  
# Retrieve the Tenant Virtual Network configuration  
$Vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri  -ResourceId "Contoso_Vnet1"   
  
# Display in JSON format  
$Vnet | ConvertTo-Json -Depth 4   
  
# Retrieve the Tenant Virtual Subnet configuration  
$RoutingSubnet = Get-NetworkControllerVirtualSubnet -ConnectionUri $uri  -ResourceId "Contoso_WebTier" -VirtualNetworkID $vnet.ResourceId   
  
# Display in JSON format  
$RoutingSubnet | ConvertTo-Json -Depth 4   
  
```  
Paso 3: Crear un objeto JSON de puerta de enlace virtual y agregarlo al controlador de red.  
  
```  
# Create a new object for Tenant Virtual Gateway  
$VirtualGWProperties = New-Object Microsoft.Windows.NetworkController.VirtualGatewayProperties   
  
# Update Gateway Pool reference  
$VirtualGWProperties.GatewayPools = @()   
$VirtualGWProperties.GatewayPools += $gwPool   
  
# Specify the Virtual Subnet that is to be used for routing between the gateway and Virtual Network   
$VirtualGWProperties.GatewaySubnets = @()   
$VirtualGWProperties.GatewaySubnets += $RoutingSubnet   
  
# Update the rest of the Virtual Gateway object properties  
$VirtualGWProperties.RoutingType = "Dynamic"   
$VirtualGWProperties.NetworkConnections = @()   
$VirtualGWProperties.BgpRouters = @()   
  
# Add the new Virtual Gateway for tenant   
$virtualGW = New-NetworkControllerVirtualGateway -ConnectionUri $uri  -ResourceId "Contoso_VirtualGW" -Properties $VirtualGWProperties -Force   
  
```  
  
## <a name="bkmk_s2s1"></a>Agregar una conexión de red VPN de sitio a sitio para un inquilino (IPsec, GRE o L3)  
  
Puedes crear una conexión VPN para sitios con IPsec, GRE o Layer 3 (L3) reenvío mediante el uso de los siguientes ejemplos para cada tipo de puerta de enlace.  
  
### <a name="ipsec-vpn-site-to-site-network-connection"></a>Conexión de red del sitio a sitio VPN IPsec  
  
Crea un objeto JSON de conexión de red y agregarlo al controlador de red.  
  
```  
# Create a new object for Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "P@ssw0rd"   
  
$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "SHA256128"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "DES3"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 1233   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   
  
$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 1234   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   
  
# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.1.10.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "10.127.134.121"   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force   
  
  
```  
### <a name="gre-vpn-site-to-site-network-connection"></a>Conexión de red del sitio a sitio GRE VPN  
  
Crea un objeto JSON de conexión de red y agregarlo al controlador de red.  
  
```  
# Create a new object for the Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "GRE"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration   
$nwConnectionProperties.GreConfiguration.GreKey = 1234   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.2.20.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "10.127.134.122"   
  
# L3 specific configuration (leave blank for GRE)  
$nwConnectionProperties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration   
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_GreGW" -Properties $nwConnectionProperties -Force   
  
```  
### <a name="l3-forwarding-network-connection"></a>L3 Reenvío de conexión de red  
  
Para configurar una conexión de red de reenvío L3, también debes configurar una red lógica correspondiente.   
  
Paso 1: Configurar una red lógica para el L3 reenvío de conexión de red.  
  
```  
# Create a new object for the Logical Network to be used for L3 Forwarding  
$lnProperties = New-Object Microsoft.Windows.NetworkController.LogicalNetworkProperties  
  
$lnProperties.NetworkVirtualizationEnabled = $false  
$lnProperties.Subnets = @()  
  
# Create a new object for the Logical Subnet to be used for L3 Forwarding and update properties  
$logicalsubnet = New-Object Microsoft.Windows.NetworkController.LogicalSubnet  
$logicalsubnet.ResourceId = "Contoso_L3_Subnet"  
$logicalsubnet.Properties = New-Object Microsoft.Windows.NetworkController.LogicalSubnetProperties  
$logicalsubnet.Properties.VlanID = 1001  
$logicalsubnet.Properties.AddressPrefix = "10.127.134.0/25"  
$logicalsubnet.Properties.DefaultGateways = "10.127.134.1"  
  
$lnProperties.Subnets += $logicalsubnet  
  
# Add the new Logical Network to Network Controller  
$vlanNetwork = New-NetworkControllerLogicalNetwork -ConnectionUri $uri -ResourceId "Contoso_L3_Network" -Properties $lnProperties -Force  
  
```  
Paso 2: Crear un objeto JSON de conexión de red y agregarlo al controlador de red.  
  
```  
# Create a new object for the Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "L3"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# GRE specific configuration (leave blank for L3)  
$nwConnectionProperties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration   
$nwConnectionProperties.L3Configuration.VlanSubnet = $vlanNetwork.properties.Subnets[0]   
  
$nwConnectionProperties.IPAddresses = @()   
$localIPAddress = New-Object Microsoft.Windows.NetworkController.CidrIPAddress   
$localIPAddress.IPAddress = "10.127.134.55"   
$localIPAddress.PrefixLength = 25   
$nwConnectionProperties.IPAddresses += $localIPAddress   
  
$nwConnectionProperties.PeerIPAddresses = @("10.127.134.65")   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.2.20.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_L3GW" -Properties $nwConnectionProperties -Force   
  
```  
  
## <a name="bkmk_bgp1"></a>Configurar la puerta de enlace como enrutador BGP  
  
Puedes usar los siguientes scripts de ejemplo para configurar la puerta de enlace como un enrutador de protocolo de puerta de enlace de borde (BGP).  
  
### <a name="add-a-bgp-router-for-the-tenant"></a>Agregar un enrutador BGP del inquilino  
  
Crea un objeto de JSON BGP enrutador y agregarlo al controlador de red.  
  
```  
# Create a new object for the Tenant BGP Router  
$bgpRouterproperties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties   
  
# Update the BGP Router properties  
$bgpRouterproperties.ExtAsNumber = "0.64512"   
$bgpRouterproperties.RouterId = "192.168.0.2"   
$bgpRouterproperties.RouterIP = @("192.168.0.2")   
  
# Add the new BGP Router for the tenant  
$bgpRouter = New-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_BgpRouter1" -Properties $bgpRouterProperties -Force   
   
  
```  
### <a name="add-a-bgp-peer-for-this-tenant-corresponding-to-the-site-to-site-vpn-network-connection-added-above"></a>Agrega un sistema del mismo nivel BGP para este inquilino, correspondientes a la conexión de red VPN de sitio a sitio agregado  
  
Crea un objeto de JSON BGP del mismo nivel y agregarlo al controlador de red.  
  
```  
# Create a new object for Tenant BGP Peer  
$bgpPeerProperties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties   
  
# Update the BGP Peer properties  
$bgpPeerProperties.PeerIpAddress = "14.1.10.1"   
$bgpPeerProperties.AsNumber = 64521   
$bgpPeerProperties.ExtAsNumber = "0.64521"   
  
# Add the new BGP Peer for tenant  
New-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -BgpRouterName $bgpRouter.ResourceId -ResourceId "Contoso_IPSec_Peer" -Properties $bgpPeerProperties -Force   
  
```  
## <a name="bkmk_all3"></a>Configurar una puerta de enlace con todos los tipos de conexión tres (IPsec, GRE, L3) y BGP  
Opcionalmente, puedes combinar todos los pasos anteriores y configurar una puerta de enlace de inquilino virtual con todas las opciones de conexión tres:   
  
```  
# Create a new Virtual Gateway Properties type object  
$VirtualGWProperties = New-Object Microsoft.Windows.NetworkController.VirtualGatewayProperties  
  
# Update GatewayPool reference  
$VirtualGWProperties.GatewayPools = @()  
$VirtualGWProperties.GatewayPools += $gwPool  
  
# Specify the Virtual Subnet that is to be used for routing between GW and VNET  
$VirtualGWProperties.GatewaySubnets = @()  
$VirtualGWProperties.GatewaySubnets += $RoutingSubnet  
  
# Update some basic properties  
$VirtualGWProperties.RoutingType = "Dynamic"  
  
# Update Network Connection object(s)  
$VirtualGWProperties.NetworkConnections = @()  
  
# IPSec Connection configuration  
$ipSecConnection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$ipSecConnection.ResourceId = "Contoso_IPSecGW"  
$ipSecConnection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$ipSecConnection.Properties.ConnectionType = "IPSec"  
$ipSecConnection.Properties.OutboundKiloBitsPerSecond = 10000  
$ipSecConnection.Properties.InboundKiloBitsPerSecond = 10000  
  
$ipSecConnection.Properties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration  
  
$ipSecConnection.Properties.IpSecConfiguration.AuthenticationMethod = "PSK"  
$ipSecConnection.Properties.IpSecConfiguration.SharedSecret = "P@ssw0rd"  
  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode  
  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "SHA256128"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "DES3"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 1233  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000  
  
$ipSecConnection.Properties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode  
  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 1234  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000  
  
$ipSecConnection.Properties.IPAddresses = @()  
$ipSecConnection.Properties.PeerIPAddresses = @()  
  
$ipSecConnection.Properties.Routes = @()  
  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.1.10.1/32"  
$ipv4Route.metric = 10  
$ipSecConnection.Properties.Routes += $ipv4Route  
  
$ipSecConnection.Properties.DestinationIPAddress = "10.127.134.121"  
  
# GRE Connection configuration  
$greConnection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$greConnection.ResourceId = "Contoso_GreGW"  
  
$greConnection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$greConnection.Properties.ConnectionType = "GRE"  
$greConnection.Properties.OutboundKiloBitsPerSecond = 10000  
$greConnection.Properties.InboundKiloBitsPerSecond = 10000  
  
$greConnection.Properties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration  
$greConnection.Properties.GreConfiguration.GreKey = 1234  
  
$greConnection.Properties.IPAddresses = @()  
$greConnection.Properties.PeerIPAddresses = @()  
  
$greConnection.Properties.Routes = @()  
  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.2.20.1/32"  
$ipv4Route.metric = 10  
$greConnection.Properties.Routes += $ipv4Route  
  
$greConnection.Properties.DestinationIPAddress = "10.127.134.122"  
  
$greConnection.Properties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration  
  
# L3 Forwarding connection configuration  
$l3Connection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$l3Connection.ResourceId = "Contoso_L3GW"  
  
$l3Connection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$l3Connection.Properties.ConnectionType = "L3"  
$l3Connection.Properties.OutboundKiloBitsPerSecond = 10000  
$l3Connection.Properties.InboundKiloBitsPerSecond = 10000  
  
$l3Connection.Properties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration  
$l3Connection.Properties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration  
$l3Connection.Properties.L3Configuration.VlanSubnet = $vlanNetwork.properties.Subnets[0]  
  
$l3Connection.Properties.IPAddresses = @()  
$localIPAddress = New-Object Microsoft.Windows.NetworkController.CidrIPAddress  
$localIPAddress.IPAddress = "10.127.134.55"  
$localIPAddress.PrefixLength = 25  
$l3Connection.Properties.IPAddresses += $localIPAddress  
  
$l3Connection.Properties.PeerIPAddresses = @("10.127.134.65")  
  
$l3Connection.Properties.Routes = @()  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.2.20.1/32"  
$ipv4Route.metric = 10  
$l3Connection.Properties.Routes += $ipv4Route  
  
# Update BGP Router Object  
$VirtualGWProperties.BgpRouters = @()  
  
$bgpRouter = New-Object Microsoft.Windows.NetworkController.VGwBgpRouter  
$bgpRouter.ResourceId = "Contoso_BgpRouter1"  
$bgpRouter.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties  
  
$bgpRouter.Properties.ExtAsNumber = "0.64512"  
$bgpRouter.Properties.RouterId = "192.168.0.2"  
$bgpRouter.Properties.RouterIP = @("192.168.0.2")  
  
$bgpRouter.Properties.BgpPeers = @()  
  
# Create BGP Peer Object(s)  
# BGP Peer for IPSec Connection  
$bgpPeer_IPSec = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_IPSec.ResourceId = "Contoso_IPSec_Peer"  
  
$bgpPeer_IPSec.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_IPSec.Properties.PeerIpAddress = "14.1.10.1"  
$bgpPeer_IPSec.Properties.AsNumber = 64521  
$bgpPeer_IPSec.Properties.ExtAsNumber = "0.64521"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_IPSec  
  
# BGP Peer for GRE Connection  
$bgpPeer_Gre = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_Gre.ResourceId = "Contoso_Gre_Peer"  
  
$bgpPeer_Gre.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_Gre.Properties.PeerIpAddress = "14.2.20.1"  
$bgpPeer_Gre.Properties.AsNumber = 64522  
$bgpPeer_Gre.Properties.ExtAsNumber = "0.64522"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_Gre  
  
# BGP Peer for L3 Connection  
$bgpPeer_L3 = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_L3.ResourceId = "Contoso_L3_Peer"  
  
$bgpPeer_L3.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_L3.Properties.PeerIpAddress = "14.3.30.1"  
$bgpPeer_L3.Properties.AsNumber = 64523  
$bgpPeer_L3.Properties.ExtAsNumber = "0.64523"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_L3  
  
$VirtualGWProperties.BgpRouters += $bgpRouter  
  
# Finally Add the new Virtual Gateway for tenant  
New-NetworkControllerVirtualGateway -ConnectionUri $uri  -ResourceId "Contoso_VirtualGW" -Properties $VirtualGWProperties -Force  
  
```  
## <a name="bkmk_modify"></a>Modificar o quitar una puerta de enlace para una red Virtual  
  
Puedes usar los siguientes scripts de ejemplo para modificar o quitar una puerta de enlace existente.  
  
### <a name="modify-the-configuration-of-an-existing-gateway"></a>Modificar la configuración de una puerta de enlace existente  
Puedes usar los siguientes comandos para modificar una puerta de enlace existente.  
  
Paso 1: Recuperar la configuración del componente y guardarla en una variable  
  
```  
$nwConnection = Get-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW"  
```  
Paso 2: Navegar por la estructura de variable para llegar a la propiedad necesaria y se establece en el valor de actualizaciones  
  
```  
$nwConnection.properties.IpSecConfiguration.SharedSecret = "C0mplexP@ssW0rd"  
```  
Paso 3: Agregar la configuración modificada para reemplazar la configuración anterior en el controlador de red  
  
```  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId $nwConnection.ResourceId -Properties $nwConnection.Properties -Force  
```  
### <a name="remove-a-gateway"></a>Quitar una puerta de enlace  
Puedes usar los siguientes comandos de Windows PowerShell para quitar características individuales de la puerta de enlace o toda la puerta de enlace.  
  
#### <a name="remove-a-network-connection"></a>Quitar una conexión de red  
```  
Remove-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW" -Force  
```  
#### <a name="remove-a-bgp-peer"></a>Quitar un sistema del mismo nivel BGP  
```  
Remove-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -BgpRouterName "Contoso_BgpRouter1" -ResourceId "Contoso_IPSec_Peer" -Force  
```  
#### <a name="remove-a-bgp-router"></a>Quitar un enrutador BGP  
```  
Remove-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_BgpRouter1" -Force  
```  
#### <a name="remove-a-gateway"></a>Quitar una puerta de enlace  
```  
Remove-NetworkControllerVirtualGateway -ConnectionUri $uri -ResourceId "Contoso_VirtualGW" -Force   
```  
  
  


