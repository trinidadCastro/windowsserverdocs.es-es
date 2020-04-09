---
title: Clústeres invitados en una red virtual
description: Las máquinas virtuales conectadas a una red virtual solo pueden usar las direcciones IP asignadas por la controladora de red para comunicarse en la red.  Las tecnologías de agrupación en clústeres que requieren una dirección IP flotante, como los clústeres de conmutación por error de Microsoft, requieren algunos pasos adicionales para que funcionen correctamente.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: AnirbanPaul
ms.date: 08/26/2018
ms.openlocfilehash: 6889b58f5d49a4932ef8277b11e1002e85606f3f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854458"
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clústeres invitados en una red virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Las máquinas virtuales conectadas a una red virtual solo pueden usar las direcciones IP asignadas por la controladora de red para comunicarse en la red.  Las tecnologías de agrupación en clústeres que requieren una dirección IP flotante, como los clústeres de conmutación por error de Microsoft, requieren algunos pasos adicionales para que funcionen correctamente.

El método para hacer que la IP flotante sea accesible es usar una Load Balancer de software \(SLB\) IP virtual \(VIP\).  El equilibrador de carga de software debe configurarse con un sondeo de estado en un puerto en esa dirección IP para que SLB dirija el tráfico a la máquina que actualmente tiene esa IP.


## <a name="example-load-balancer-configuration"></a>Ejemplo: configuración del equilibrador de carga

En este ejemplo se da por supuesto que ya ha creado las máquinas virtuales que se convertirán en nodos de clúster y las asociará a un Virtual Network.  Para obtener instrucciones, consulte [creación de una máquina virtual y conexión a un inquilino Virtual Network o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

En este ejemplo se creará una dirección IP virtual (192.168.2.100) para representar la dirección IP flotante del clúster y se configurará un sondeo de estado para supervisar el puerto TCP 59999 para determinar qué nodo es el activo.

1. Seleccione la dirección VIP.<p>Prepárese mediante la asignación de una dirección IP de VIP, que puede ser cualquier dirección no usada o reservada en la misma subred que los nodos de clúster.  La VIP debe coincidir con la dirección flotante del clúster.

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. Cree el objeto de propiedades del equilibrador de carga.

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. Cree una dirección IP de Front\-end.

   ```PowerShell
   $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
   $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
   $FrontEnd.resourceId = "Frontend1"
   $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
   $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
   $FrontEnd.properties.privateIPAddress = $VIP
   $FrontEnd.properties.privateIPAllocationMethod = "Static"
   ```

4. Cree un grupo de back\-end para que contenga los nodos de clúster.

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. Agregue un sondeo para detectar en qué nodo del clúster está actualmente activa la dirección flotante. 

   >[!NOTE]
   >La consulta de sondeo en la dirección permanente de la máquina virtual en el puerto que se define a continuación.  El puerto solo debe responder en el nodo activo. 

   ```PowerShell
   $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
   $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

   $lbprobe.ResourceId = "Probe1"
   $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
   $lbprobe.properties.protocol = "TCP"
   $lbprobe.properties.port = "59999"
   $lbprobe.properties.IntervalInSeconds = 5
   $lbprobe.properties.NumberOfProbes = 11
   ```

6. Agregue las reglas de equilibrio de carga para el puerto TCP 1433.<p>Puede modificar el protocolo y el puerto según sea necesario.  También puede repetir este paso varias veces para puertos adicionales y protcols en esta VIP.  Es importante que EnableFloatingIP esté establecido en $true porque esto indica al equilibrador de carga que envíe el paquete al nodo con la VIP original en su lugar.

   ```PowerShell
   $LoadBalancerProperties.loadbalancingRules += $lbrule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $lbrule.properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $lbrule.ResourceId = "Rules1"

   $lbrule.properties.frontendipconfigurations += $FrontEnd
   $lbrule.properties.backendaddresspool = $BackEnd 
   $lbrule.properties.protocol = "TCP"
   $lbrule.properties.frontendPort = $lbrule.properties.backendPort = 1433 
   $lbrule.properties.IdleTimeoutInMinutes = 4
   $lbrule.properties.EnableFloatingIP = $true
   $lbrule.properties.Probe = $lbprobe
   ```

7. Cree el equilibrador de carga en la controladora de red.

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. Agregue los nodos de clúster al grupo de back-end.<p>Puede agregar tantos nodos al grupo como necesite para el clúster.

   ```PowerShell
   # Cluster Node 1

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force
   ```

   Una vez que haya creado el equilibrador de carga y agregado las interfaces de red al grupo de back-end, estará listo para configurar el clúster.  

9. Opta Si usa un clúster de conmutación por error de Microsoft, continúe con el ejemplo siguiente. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Ejemplo 2: configurar un clúster de conmutación por error de Microsoft

Puede usar los pasos siguientes para configurar un clúster de conmutación por error.

1. Instalar y configurar las propiedades de un clúster de conmutación por error.

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = "192.168.2.100" 

   $nodes = @("DB1", "DB2")
   ```

2. Cree el clúster en un nodo.

   ```PowerShell
   New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]
   ```

3. Detenga el recurso de clúster.

   ```PowerShell
   Stop-ClusterResource "Cluster Name" 
   ```

4. Establezca la dirección IP del clúster y el puerto de sondeo.<p>La dirección IP debe coincidir con la dirección IP de front-end utilizada en el ejemplo anterior, y el puerto de sondeo debe coincidir con el puerto de sondeo en el ejemplo anterior.

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. Inicie los recursos del clúster.

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. Agregue los nodos restantes.

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**El clúster está activo.**_ El tráfico dirigido a la VIP en el puerto especificado se dirige al nodo activo.

---