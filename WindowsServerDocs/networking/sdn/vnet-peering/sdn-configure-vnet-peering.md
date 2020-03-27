---
title: Configurar el emparejamiento de la red virtual
description: La configuración del emparejamiento de redes virtuales implica la creación de dos redes virtuales que se van a emparejar.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: lizross
author: eross-msft
ms.date: 08/08/2018
ms.openlocfilehash: 4ea035d80a32e245edc4633ee14e98b9d1153fff
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309737"
---
# <a name="configure-virtual-network-peering"></a>Configurar el emparejamiento de la red virtual

>Se aplica a: Windows Server

En este procedimiento, usará Windows PowerShell para crear dos redes virtuales, cada una con una subred. A continuación, configure el emparejamiento entre las dos redes virtuales para habilitar la conectividad entre ellos.

- [Paso 1. Crear la primera red virtual](#step-1-create-the-first-virtual-network)

- [Paso 2. Creación de la segunda red virtual](#step-2-create-the-second-virtual-network)

- [Paso 3. Configuración del emparejamiento desde la primera red virtual a la segunda red virtual](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [Paso 4. Configurar el emparejamiento desde la segunda red virtual a la primera red virtual](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>Recuerde actualizar las propiedades de su entorno.

## <a name="step-1-create-the-first-virtual-network"></a>Paso 1. Crear la primera red virtual

En este paso, usará Windows PowerShell para buscar la red lógica del proveedor de HNV para crear la primera red virtual con una subred. El siguiente script de ejemplo crea una red virtual de Contoso con una subred.

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

## <a name="step-2-create-the-second-virtual-network"></a>Paso 2. Creación de la segunda red virtual

En este paso, creará una segunda red virtual con una subred. El siguiente script de ejemplo crea una red virtual de Woodgrove con una subred.

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

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>Paso 3. Configuración del emparejamiento desde la primera red virtual a la segunda red virtual

En este paso, configurará el emparejamiento entre la primera red virtual y la segunda red virtual que creó en los dos pasos anteriores. En el siguiente script de ejemplo se establece el emparejamiento de redes virtuales desde **Contoso_vnet1** a **Woodgrove_vnet1**.

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
>Después de crear este emparejamiento, el estado de la red virtual muestra **iniciado**.

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>Paso 4. Configurar el emparejamiento desde la segunda red virtual a la primera red virtual

En este paso, configurará el emparejamiento entre la segunda red virtual y la primera red virtual que creó en los pasos 1 y 2 anteriores. En el siguiente script de ejemplo se establece el emparejamiento de redes virtuales desde **Woodgrove_vnet1** a **Contoso_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network's gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network's gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

Después de crear este emparejamiento, el estado de emparejamiento de Vnet muestra **conectado** para ambos equipos del mismo nivel. Ahora, las máquinas virtuales de una red virtual pueden comunicarse con las máquinas virtuales de la red virtual emparejada.

---