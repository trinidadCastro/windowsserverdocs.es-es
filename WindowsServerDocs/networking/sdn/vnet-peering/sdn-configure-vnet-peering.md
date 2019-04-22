---
title: Configurar el emparejamiento de la red virtual
description: Configurar el emparejamiento de redes virtuales implica la creación de dos redes virtuales que obtengan emparejadas.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 3ef3db879080e3372e7b287dcc55ae052c1fe109
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816396"
---
# <a name="configure-virtual-network-peering"></a>Configurar el emparejamiento de la red virtual

>Se aplica a: Windows Server

En este procedimiento, usa Windows PowerShell para crear dos redes virtuales, cada uno con una subred. A continuación, se configura el emparejamiento entre las dos redes virtuales para habilitar la conectividad entre ellos.

- [Paso 1. Crear la primera red virtual](#step-1-create-the-first-virtual-network)

- [Paso 2. Cree la segunda red virtual](#step-2-create-the-second-virtual-network)

- [Paso 3. Configurar el emparejamiento de la primera red virtual a la segunda red virtual](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [Paso 4. Configurar el emparejamiento de la segunda red virtual a la primera red virtual](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>No olvide actualizar las propiedades para su entorno.

## <a name="step-1-create-the-first-virtual-network"></a>Paso 1. Crear la primera red virtual

En este paso, usará Windows PowerShell encontrar la red lógica del proveedor HNV para crear la primera red virtual con una subred. El siguiente script de ejemplo crea la red virtual de Contoso con una subred.

``` PowerShell
#Find the HNV Provider Logical Network  

$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"
$uri=”https://restserver”  

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-2-create-the-second-virtual-network"></a>Paso 2. Cree la segunda red virtual

En este paso, creará una segunda red virtual con una subred. El siguiente script de ejemplo crea la red virtual de Woodgrove Bank, con una subred.

``` PowerShell

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Woodgrove"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
$uri=”https://restserver”

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.2.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Woodgrove_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>Paso 3. Configurar el emparejamiento de la primera red virtual a la segunda red virtual

En este paso, configure el emparejamiento entre la primera red virtual y la segunda red virtual que creó en los dos pasos anteriores. El siguiente script de ejemplo establece el emparejamiento de redes virtuales de **Contoso_vnet1** a **Woodgrove_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties
$vnet2 = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Woodgrove_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2

#Indicate whether communication between the two virtual networks
$peeringProperties.allowVirtualnetworkAccess = $true

#Indicates whether forwarded traffic is allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true

#Indicates whether the peer virtual network can access this virtual networks gateway
$peeringProperties.allowGatewayTransit = $false

#Indicates whether this virtual network uses peer virtual networks gateway
$peeringProperties.useRemoteGateways =$false

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Contoso_vnet1” -ResourceId “ContosotoWoodgrove” -Properties $peeringProperties

```

>[!IMPORTANT]
>Después de crear este emparejamiento, se muestra el estado de la red virtual **iniciado**.

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>Paso 4. Configurar el emparejamiento de la segunda red virtual a la primera red virtual

En este paso, configure el emparejamiento entre la segunda red virtual y la primera red virtual que creó en los pasos 1 y 2 anteriores. El siguiente script de ejemplo establece el emparejamiento de redes virtuales de **Woodgrove_vnet1** a **Contoso_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network’s gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network’s gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

Después de crear un emparejamiento, el estado de emparejamiento de vnet muestra **conectado** para los pares de ambos. Ahora, las máquinas virtuales de una red virtual puede comunicarse con máquinas virtuales en la red virtual emparejada.

---