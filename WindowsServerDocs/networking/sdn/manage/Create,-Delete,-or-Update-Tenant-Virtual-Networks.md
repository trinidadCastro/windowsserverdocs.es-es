---
title: Crear, eliminar o actualizar el inquilino de redes virtuales
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ef30dcc31593e15c36f846cf6d64afcd4b85f19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Crear, eliminar o actualizar el inquilino de redes virtuales

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprender a crear, eliminar y actualizar redes virtuales de Hyper-V red virtualización después de implementar Software definido de redes (SDN).  
  
Usar la virtualización de la red de Hyper-V, puede aislar inquilino redes para que cada red de inquilino es una entidad completamente independiente sin posibilidad entre conexión a menos que configure las cargas de trabajo de acceso público.  
  
Puedes crear nuevas redes virtuales de inquilinos, puedes modificar las redes virtuales existente y, si un inquilino ya no necesita ciertos recursos, o si el inquilino ya no es tu cliente, puedes eliminar a inquilino redes virtuales.  
  
## <a name="create-a-new-virtual-network"></a>Crear una nueva red Virtual  
  
Cuando creas una red Virtual para un inquilino, se coloca dentro de un dominio único de enrutamiento en el host de Hyper-V.  
  
Siguiente es los pasos para crear una nueva red Virtual.  
  
1. Identificar los prefijos de dirección IP desde el que quieras para crear las subredes virtuales.   
2. Identificar la red de proveedor lógicos en los que se aplica un túnel el tráfico del inquilino.   
3. Crear al menos una subred virtual para cada prefijo IP que definiste en el paso 1.   
  
>[!NOTE]  
>Debajo de cada red virtual hay al menos una subred virtual. Virtuales subredes se definen mediante un prefijo de IP y hacer referencia a una lista de Control de acceso definida previamente.  
  
Opcionalmente, una vez completados estos pasos, puedes también agregar las listas de control de acceso creado anteriormente a las subredes virtuales, o agregar conectividad de puerta de enlace de inquilinos.    
  
La tabla siguiente incluye subred identificadores de ejemplo y prefijos de inquilinos ficticios dos. El inquilino Fabrikam tiene dos subredes virtuales, mientras que el inquilino de Contoso tiene tres subredes virtuales.  
  
  
  
Nombre de inquilino  |Id. de subred virtual  |Prefijo de subred virtual    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
El siguiente script de ejemplo usa los comandos de Windows PowerShell exportados desde el **NetworkController** módulo para crear redes virtuales de Contoso y de una subred:   
  
```  
import-module networkcontroller  
$URI = "https://ncrest.contoso.local"  
  
#Find the HNV Provider Logical Network  
  
$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   
  
#Find the Access Control List to user per virtual subnet  
  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
#Create the Virtual Subnet  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_WebTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"  
  
#Create the Virtual Network  
  
$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties  
  
  
```  
  
## <a name="modify-an-existing-virtual-network"></a>Modificar una red Virtual existente  
Puedes usar Windows PowerShell para actualizar una subred Virtual existente o red.   
  
Cuando se ejecuta el siguiente script de ejemplo, los recursos actualizados se COLOCAN simplemente al controlador de red con el mismo identificador de recursos. Si tu inquilino de Contoso quiere agregar una nueva (24.30.2.0 # de subred virtual/24) a su red virtual, puedes o el Administrador de Contoso puede utilizar el siguiente script.  
  
```  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
$vnet = Get-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri  
  
$vnet.properties.AddressSpace.AddressPrefixes += "24.30.2.0/24"  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_DBTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
  
$vnet.properties.Subnets += $vsubnet  
  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -properties $vnet.properties  
  
```  
  
## <a name="delete-a-virtual-network"></a>Eliminar una red Virtual  
  
Puedes usar Windows PowerShell para eliminar una red Virtual.  
  
El siguiente ejemplo de Windows PowerShell, elimina a un inquilino de red Virtual emitiendo un eliminar HTTP como el URI del identificador de recursos.  
  
    Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  


