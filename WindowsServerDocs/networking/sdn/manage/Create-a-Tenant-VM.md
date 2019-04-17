---
title: Crear una máquina virtual y conectarse a un inquilino Virtual VLAN o red
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 001eb3efa073e4ffbdfad41f88949303159a7274
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>Crear una máquina virtual y conectarse a un inquilino Virtual VLAN o red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para crear un inquilino virtual \(VM\) del equipo y conectar la máquina virtual a una red virtuales que has creado con la virtualización de Hyper-V red o a un virtual Local red de área \(VLAN\). 

Este tema contiene las siguientes secciones.

- [Crear una máquina virtual y conectarse a una red Virtual mediante los cmdlets de controlador de red de Windows PowerShell](#bkmk_vn)
- [Crear una máquina virtual y conectarse a una VLAN mediante NetworkControllerRESTWrappers](#bkmk_vlan)

## <a name="requirements"></a>Requisitos
Antes de realizar los procedimientos descritos en las siguientes secciones, ten en cuenta los requisitos siguientes.

1. Debes crear adaptadores de red de la máquina virtual con medios estáticos \(MAC\) direcciones para que la dirección MAC de la máquina virtual no cambien durante el ciclo de vida de la máquina virtual de control de acceso. 
>[!NOTE]
>Si la dirección MAC de máquina virtual cambia durante el ciclo de vida de la máquina virtual, el controlador de red no puede configurar la directiva necesaria para el adaptador de red. Si no se configura la directiva para el adaptador de red, el adaptador de red es evitar el tráfico de red de procesamiento, y se produce un error en toda la comunicación con la red. 

2. Si la máquina virtual requiere acceso a la red al inicio, es importante que no se inicia la máquina virtual hasta después el paso de configuración final: establecer el identificador de la interfaz en la máquina virtual de puerto del adaptador de red. Si inicias la máquina virtual para completar este paso, la máquina virtual no se pueden comunicar en la red hasta que la interfaz de red se crea en el controlador de red y el controlador aplicado todas las directivas aplicables - directiva de red Virtual, listas de control de acceso \(ACLs\) y la calidad de servicio \(QoS\).

También puedes usar los procesos que se describen en este tema para la implementación de dispositivos virtuales. Con unos pasos adicionales, puedes configurar dispositivos para procesar o inspeccionar los paquetes de datos que fluyen a o desde otras máquinas virtuales en la red Virtual.

>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen los valores de ejemplo para el número de parámetros. Asegúrate de que los valores de ejemplo en estos comandos, se reemplaza con valores que son adecuados para su implementación antes de ejecutar estos comandos.  

## <a name="bkmk_vn"></a>Crear una máquina virtual y conectarse a una red Virtual mediante los cmdlets de controlador de red de Windows PowerShell

Esta sección incluye los siguientes temas.

1.  [Crear una máquina virtual con un adaptador de red de la máquina virtual que tenga la dirección MAC estática](#bkmk_create)
2.  [Obtener la red Virtual que contiene la subred a la que va a conectar el adaptador de red](#bkmk_getvn)
3.  [Crear un objeto de la interfaz de red en el controlador de red](#bkmk_object)
4.  [Obtener InstanceId para la interfaz de red desde el controlador de red](#bkmk_getinstance)
5.  [Establecer el identificador de la interfaz en la máquina virtual de Hyper-V puerto de adaptador de red](#bkmk_setinstance)
6.  [Inicia la máquina virtual](#bkmk_start)

### <a name="bkmk_create"></a>Crear una máquina virtual con un adaptador de red de la máquina virtual que tenga la dirección MAC estática

Para crear una máquina virtual con un adaptador de red que tenga la dirección MAC estática, usa el siguiente comando de ejemplo.

    
    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
    Set-VM -Name "MyVM" -ProcessorCount 4
    
    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
    

### <a name="bkmk_getvn"></a>Obtener la red Virtual que contiene la subred a la que va a conectar el adaptador de red
Asegúrate de que ya has creado una red Virtual antes de usar este comando de ejemplo. Para obtener más información, consulta [crear, eliminar o redes virtuales del inquilino de actualización](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks).

Para obtener la red Virtual, usa el siguiente comando de ejemplo.

    
    $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
    

>[!NOTE]
>Si necesitas ACL personalizadas para esta interfaz de red, a continuación, crea la ACL ahora con instrucciones en el tema [usa acceso listas de Control (ACL) para hacer fluir el tráfico de red de Datacenter administrar](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)

### <a name="bkmk_object"></a>Crear un objeto de la interfaz de red en el controlador de red

Para crear un objeto de la interfaz de red en el controlador de red, usa el siguiente comando de ejemplo.

>[!NOTE]
>Si has creado una ACL personalizada después del paso anterior, se puede usar ahora.

    
    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "001122334455" 
    $vmnicproperties.PrivateMacAllocationMethod = "Static" 
    $vmnicproperties.IsPrimary = $true 

    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = @("24.30.1.11", "24.30.1.12")
    
    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_IP1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = “24.30.1.101”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"
    
    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $vnet.Properties.Subnets[0].ResourceRef
    
    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri
    

### <a name="bkmk_getinstance"></a>Obtener InstanceId para la interfaz de red desde el controlador de red
Para obtener la InstanceId para la interfaz de red desde el controlador de red, usa el siguiente comando de ejemplo.

    
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
    

### <a name="bkmk_setinstance"></a>Establecer el identificador de la interfaz en la máquina virtual de Hyper-V puerto de adaptador de red
Para establecer el identificador de la interfaz de la máquina virtual de Hyper-V puerto de adaptador de red, usa el siguiente comando de ejemplo.

>[!NOTE]
>Debes ejecutar estos comandos en el host de Hyper-V está instalada en la máquina virtual.

    
    #Do not change the hardcoded IDs in this section, because they are fixed values and must not change.
    
    $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"
    
    $vmNics = Get-VMNetworkAdapter -VMName “MyVM”
    
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
    

### <a name="bkmk_start"></a>Inicia la máquina virtual
Para iniciar la máquina virtual, usa el siguiente comando de ejemplo.

    
    Get-VM -Name “MyVM” | Start-VM 
    
Ahora ha creado una máquina virtual, conectado la máquina virtual a un red Virtual del inquilino e inicia la máquina virtual para que sea capaz de procesar las cargas de trabajo del inquilino.

## <a name="bkmk_vlan"></a>Crear una máquina virtual y conectarse a una VLAN mediante NetworkControllerRESTWrappers

Esta sección incluye los siguientes temas.

1. [Crear la máquina virtual y asignar una dirección MAC estática](#bkmk_mac)
2. [Establecer el ID de VLAN en el adaptador de red de la máquina virtual](#bkmk_vid)
3. [Obtener la subred lógica y crear la interfaz de red](#bkmk_subnet)
4. [Establecer el InstanceId en el puerto de Hyper-V](#bkmk_instance)
5. [Inicia la máquina virtual](#bkmk_startvlan)


###<a name="bkmk_mac"></a>Crear la máquina virtual y asignar una dirección MAC estática
Para crear una máquina virtual y asignar un medio estático \(MAC\) dirección de control de acceso a la máquina virtual, puedes usar los siguientes comandos de ejemplo.

    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

    Set-VM -Name "MyVM" -ProcessorCount 4

    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 

###<a name="bkmk_vid"></a>Establecer el ID de VLAN en el adaptador de red de la máquina virtual
Para establecer el ID de VLAN en el adaptador de red, puedes usar el siguiente comando de ejemplo.


    Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123


###<a name="bkmk_subnet"></a>Obtener la subred lógica y crear la interfaz de red

Para obtener la subred lógica y crear la interfaz de red mediante la lógica de subred, puedes usar los siguientes comandos de ejemplo.


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
    $ipconfiguration.properties.PrivateIPAddress = “10.127.132.177”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"

    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $logicalnet.Properties.Subnets[0].ResourceRef

    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    $vnic = New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri

    $vnic.InstanceId
    

###<a name="bkmk_instance"></a>Establecer el InstanceId en el puerto de Hyper-V
Para establecer el InstanceId en el puerto de Hyper-V, puedes usar los siguientes comandos de ejemplo en el host de Hyper-V donde se encuentra la máquina virtual.

  
    #The hardcoded Ids in this section are fixed values and must not change.
    $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"

    $vmNics = Get-VMNetworkAdapter -VMName “MyVM”

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


###<a name="bkmk_startvlan"></a>Inicia la máquina virtual
Para iniciar la máquina virtual, puedes usar el siguiente comando de ejemplo.


    Get-VM -Name “MyVM” | Start-VM 

Ahora ha creado una máquina virtual, conectado a la máquina virtual a una VLAN e inicia la máquina virtual para que sea capaz de procesar las cargas de trabajo del inquilino.

  

