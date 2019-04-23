---
title: Crear, eliminar o actualizar la red virtual de inquilino
description: En este tema, aprenderá a crear, eliminar y actualizar las redes virtuales de Hyper-V Network Virtualization después de implementar redes definidas por Software (SDN). Virtualización de red de Hyper-V le ayuda a aislar las redes de inquilinos para que cada red de inquilinos es una entidad independiente. Cada entidad no tiene ninguna posibilidad de conexión cruzada a menos que configure las cargas de trabajo de acceso público.
manager: dougkim
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
ms.date: 08/24/2018
ms.openlocfilehash: a125ec220b4769a57a6be30f1425283afb7f0fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838356"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Crear, eliminar o actualizar redes virtuales de inquilinos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, aprenderá a crear, eliminar y actualizar las redes virtuales de Hyper-V Network Virtualization después de implementar redes definidas por Software (SDN). Virtualización de red de Hyper-V le ayuda a aislar las redes de inquilinos para que cada red de inquilinos es una entidad independiente. Cada entidad no tiene ninguna posibilidad de conexión cruzada a menos que configure las cargas de trabajo de acceso público.   
  
## <a name="create-a-new-virtual-network"></a>Crear una nueva red virtual  
Creación de una red virtual para un inquilino, coloca dentro de un único dominio de enrutamiento en el host de Hyper-V. Debajo de cada red virtual, hay al menos una subred virtual. Subredes virtuales obtengan definidas por un prefijo IP y hacer referencia a una ACL definida previamente.  

Los pasos para crear una nueva red virtual son:

1. Identificar los prefijos de dirección IP desde el que desea crear las subredes virtuales.   
2. Identifique la red lógica del proveedor en el que se abren el tráfico de inquilinos.   
3. Cree al menos una subred virtual para cada prefijo IP que identificó en el paso 1. 
4. (Opcional) Agregar las ACL creadas anteriormente a las subredes virtuales o conectividad de puerta de enlace para los inquilinos. 

En la tabla siguiente incluye los identificadores de subred de ejemplo y los prefijos para dos inquilinos ficticios. El inquilino Fabrikam tiene dos subredes virtuales, mientras que el inquilino Contoso tiene tres subredes virtuales.  
 
  
Nombre del inquilino  |Id. de subred virtual  |Prefijo de subred virtual    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
El siguiente script de ejemplo usa los comandos de Windows PowerShell exportados desde el **NetworkController** módulo para crear la red virtual de Contoso y una subred:   
  
```Powershell  
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
Puede usar Windows PowerShell para actualizar una red o subred Virtual existente.   
  
Al ejecutar el siguiente script de ejemplo, los recursos actualizados están SENCILLAMENTE a la controladora de red con el mismo identificador de recurso. Si el inquilino Contoso desea agregar una nueva subred virtual (24.30.2.0/24) a su red virtual, usted o el Administrador de Contoso puede usar el siguiente script.  
  
```PowerShell  
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
  
Puede usar Windows PowerShell para eliminar una red Virtual.  
  
El siguiente ejemplo de Windows PowerShell elimina un red Virtual de inquilino emitiendo un HTTP delete al URI del identificador de recurso de  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

