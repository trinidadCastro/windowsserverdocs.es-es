---
title: Creación, eliminación o actualización de una red virtual de inquilino
description: En este tema, aprenderá a crear, eliminar y actualizar redes virtuales de virtualización de red de Hyper-V después de implementar las redes definidas por software (SDN). Virtualización de red de Hyper-V le ayuda a aislar las redes de inquilinos para que cada red de inquilinos sea una entidad independiente. Cada entidad no tiene ninguna posibilidad entre conexiones a menos que configure cargas de trabajo de acceso público.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/24/2018
ms.openlocfilehash: acb663cd33d015c1ce96241abffd4ca260cc5559
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854528"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Crear, eliminar o actualizar redes virtuales de inquilinos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, aprenderá a crear, eliminar y actualizar redes virtuales de virtualización de red de Hyper-V después de implementar las redes definidas por software (SDN). Virtualización de red de Hyper-V le ayuda a aislar las redes de inquilinos para que cada red de inquilinos sea una entidad independiente. Cada entidad no tiene ninguna posibilidad entre conexiones a menos que configure cargas de trabajo de acceso público.   
  
## <a name="create-a-new-virtual-network"></a>Crear una nueva red virtual  
La creación de una red virtual para un inquilino la coloca dentro de un dominio de enrutamiento único en el host de Hyper-V. Debajo de cada red virtual, hay al menos una subred virtual. Las subredes virtuales se definen mediante un prefijo IP y hacen referencia a una ACL definida previamente.  

Los pasos para crear una nueva red virtual son:

1. Identifique los prefijos de dirección IP desde los que desea crear las subredes virtuales.   
2. Identifique la red del proveedor lógico en el que se tuneliza el tráfico de inquilinos.   
3. Cree al menos una subred virtual para cada prefijo IP que identificó en el paso 1. 
4. Opta Agregue las ACL creadas anteriormente a las subredes virtuales o agregue conectividad de puerta de enlace para los inquilinos. 

En la tabla siguiente se incluyen identificadores y prefijos de subred de ejemplo para dos inquilinos ficticios. El inquilino Fabrikam tiene dos subredes virtuales, mientras que el inquilino de Contoso tiene tres subredes virtuales.  
 
  
Nombre de inquilino  |Id. de subred virtual  |Prefijo de subred virtual    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
En el siguiente script de ejemplo se usan los comandos de Windows PowerShell exportados desde el módulo **NetworkController** para crear la red virtual de Contoso y una subred:   
  
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
  
## <a name="modify-an-existing-virtual-network"></a>Modificar un Virtual Network existente  
Puede usar Windows PowerShell para actualizar una subred o red virtual existente.   
  
Al ejecutar el siguiente script de ejemplo, los recursos actualizados simplemente se colocan en el controlador de red con el mismo identificador de recurso. Si el inquilino contoso desea agregar una nueva subred virtual (24.30.2.0/24) a su red virtual, usted o el administrador de Contoso pueden usar el script siguiente.  
  
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
  
## <a name="delete-a-virtual-network"></a>Eliminar una red virtual  
  
Puede usar Windows PowerShell para eliminar un Virtual Network.  
  
En el siguiente ejemplo de Windows PowerShell se elimina un inquilino Virtual Network emitiendo HTTP DELETE en el URI del identificador de recurso.  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

