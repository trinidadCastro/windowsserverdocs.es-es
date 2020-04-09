---
title: Creación de una máquina virtual y conexión a una red VLAN o red virtual de inquilino
description: En este tema, se muestra cómo crear una máquina virtual de inquilino y conectarla a una red virtual creada con virtualización de red de Hyper-V o a una red de área local virtual (VLAN).
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/24/2018
ms.openlocfilehash: 3949c4f10015a7fdfe4955b950b109a702553c45
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854518"
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>Creación de una máquina virtual y conexión a una red VLAN o red virtual de inquilino

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, creará una máquina virtual de inquilino y la conectará a una red virtual que creó con virtualización de red de Hyper-V o a una red de área local virtual (VLAN). Puede usar los cmdlets de la controladora de red de Windows PowerShell para conectarse a una red virtual o NetworkControllerRESTWrappers para conectarse a una VLAN.

Use los procesos descritos en este tema para implementar aplicaciones virtuales. Con unos cuantos pasos adicionales, puede configurar los dispositivos para que procesen o inspeccionen los paquetes de datos que fluyen hacia o desde otras máquinas virtuales en el Virtual Network.

Las secciones de este tema incluyen comandos de Windows PowerShell de ejemplo que contienen valores de ejemplo para muchos parámetros. Asegúrese de reemplazar los valores de ejemplo de estos comandos por los valores adecuados para su implementación antes de ejecutar estos comandos. 


## <a name="prerequisites"></a>Requisitos previos

1. Adaptadores de red de VM creados con direcciones MAC estáticas durante la vigencia de la máquina virtual.<p>Si la dirección MAC cambia durante la vigencia de la máquina virtual, la controladora de red no puede configurar la Directiva necesaria para el adaptador de red. Si no se configura la Directiva para la red, el adaptador de red no podrá procesar el tráfico de red y se producirá un error en la comunicación con la red.  

2. Si la máquina virtual requiere acceso a la red en el inicio, no inicie la máquina virtual hasta después de establecer el identificador de interfaz en el puerto del adaptador de red de la máquina virtual. Si inicia la máquina virtual antes de establecer el identificador de interfaz y la interfaz de red no existe, la máquina virtual no puede comunicarse en la red de la controladora de red y se aplican todas las directivas.

3. Si necesita ACL personalizadas para esta interfaz de red, cree la ACL ahora con las instrucciones del tema uso de [listas de Access Control (ACL) para administrar el flujo de tráfico de red del centro de](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) información.

Asegúrese de que ya ha creado un Virtual Network antes de usar este comando de ejemplo. Para obtener más información, consulte [creación, eliminación o actualización de redes virtuales de inquilino](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks).

## <a name="create-a-vm-and-connect-to-a-virtual-network-by-using-the-windows-powershell-network-controller-cmdlets"></a>Creación de una máquina virtual y conexión a un Virtual Network mediante los cmdlets de la controladora de red de Windows PowerShell


1. Cree una máquina virtual con un adaptador de red de máquina virtual con una dirección MAC estática. 

   ```PowerShell    
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
   Set-VM -Name "MyVM" -ProcessorCount 4
    
   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. Obtenga la red virtual que contiene la subred a la que desea conectar el adaptador de red.

   ```Powershell 
   $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId "Contoso_WebTier"
   ```

3. Cree un objeto de interfaz de red en la controladora de red.

   >[!TIP]
   >En este paso, utilizará la ACL personalizada.

   ```PowerShell
   $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
   $vmnicproperties.PrivateMacAddress = "001122334455" 
   $vmnicproperties.PrivateMacAllocationMethod = "Static" 
   $vmnicproperties.IsPrimary = $true 

   $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
   $vmnicproperties.DnsSettings.DnsServers = @("24.30.1.11", "24.30.1.12")
    
   $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
   $ipconfiguration.resourceid = "MyVM_IP1"
   $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
   $ipconfiguration.properties.PrivateIPAddress = "24.30.1.101"
   $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"
    
   $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $ipconfiguration.properties.subnet.ResourceRef = $vnet.Properties.Subnets[0].ResourceRef
    
   $vmnicproperties.IpConfigurations = @($ipconfiguration)
   New-NetworkControllerNetworkInterface –ResourceID "MyVM_Ethernet1" –Properties $vmnicproperties –ConnectionUri $uri
   ```

4. Obtiene el InstanceId para la interfaz de red de la controladora de red.

   ```PowerShell 
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
   ```

5. Establezca el identificador de interfaz en el puerto del adaptador de red de la máquina virtual de Hyper-V.

   >[!NOTE]
   >Debe ejecutar estos comandos en el host de Hyper-V en el que está instalada la máquina virtual.

   ```PowerShell 
   #Do not change the hardcoded IDs in this section, because they are fixed values and must not change.
    
   $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"
    
   $vmNics = Get-VMNetworkAdapter -VMName "MyVM"
    
   $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNics
    
   if ($CurrentFeature -eq $null)
   {
   $Feature = Get-VMSystemSwitchExtensionPortFeature -FeatureId $FeatureId
    
   $Feature.SettingData.ProfileId = "{$($nic.InstanceId)}"
   $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
   $Feature.SettingData.CdnLabelString = "TestCdn"
   $Feature.SettingData.CdnLabelId = 1111
   $Feature.SettingData.ProfileName = "Testprofile"
   $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
   $Feature.SettingData.VendorName = "NetworkController"
   $Feature.SettingData.ProfileData = 1
    
   Add-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNics
   }
   else
   {
   $CurrentFeature.SettingData.ProfileId = "{$($nic.InstanceId)}"
   $CurrentFeature.SettingData.ProfileData = 1
    
   Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
   }
   ```

6. Inicie la máquina virtual.

   ```PowerShell
    Get-VM -Name "MyVM" | Start-VM 
   ```

Ha creado correctamente una máquina virtual, ha conectado la máquina virtual a un inquilino Virtual Network e iniciado la máquina virtual para que pueda procesar las cargas de trabajo de inquilinos.

## <a name="create-a-vm-and-connect-to-a-vlan-by-using-networkcontrollerrestwrappers"></a>Creación de una máquina virtual y conexión a una VLAN mediante NetworkControllerRESTWrappers


1. Cree la máquina virtual y asigne una dirección MAC estática a la máquina virtual.

   ```PowerShell
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

   Set-VM -Name "MyVM" -ProcessorCount 4

   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. Establezca el identificador de VLAN en el adaptador de red de la máquina virtual.

   ```PowerShell
   Set-VMNetworkAdapterIsolation –VMName "MyVM" -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123
   ```

3. Obtenga la subred de red lógica y cree la interfaz de red. 

   ```PowerShell
    $logicalnet = get-networkcontrollerLogicalNetwork -connectionuri $uri -ResourceId "00000000-2222-1111-9999-000000000002"

    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "00-1D-C8-B7-01-02"
    $vmnicproperties.PrivateMacAllocationMethod = "Static"
    $vmnicproperties.IsPrimary = $true 
    
    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = $logicalnet.Properties.Subnets[0].DNSServers

    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_Ip1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = "10.127.132.177"
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"

    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $logicalnet.Properties.Subnets[0].ResourceRef

    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    $vnic = New-NetworkControllerNetworkInterface –ResourceID "MyVM_Ethernet1" –Properties $vmnicproperties –ConnectionUri $uri

    $vnic.InstanceId
   ```

4. Establezca InstanceId en el puerto de Hyper-V.

   ```PowerShell  
   #The hardcoded Ids in this section are fixed values and must not change.
   $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"

   $vmNics = Get-VMNetworkAdapter -VMName "MyVM"

   $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNic
        
   if ($CurrentFeature -eq $null)
   {
       $Feature = Get-VMSystemSwitchExtensionFeature -FeatureId $FeatureId
        
       $Feature.SettingData.ProfileId = "{$InstanceId}"
       $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
       $Feature.SettingData.CdnLabelString = "TestCdn"
       $Feature.SettingData.CdnLabelId = 1111
       $Feature.SettingData.ProfileName = "Testprofile"
       $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
       $Feature.SettingData.VendorName = "NetworkController"
       $Feature.SettingData.ProfileData = 1
                
       Add-VMSwitchExtensionFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNic
   }        
   else
   {
       $CurrentFeature.SettingData.ProfileId = "{$InstanceId}"
       $CurrentFeature.SettingData.ProfileData = 1

       Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
   }
   ```

5. Inicie la máquina virtual.

   ```PowerShell
   Get-VM -Name "MyVM" | Start-VM 
   ```

Ha creado correctamente una máquina virtual, ha conectado la máquina virtual a una VLAN y ha iniciado la máquina virtual para que pueda procesar las cargas de trabajo de inquilinos.

  

