---
title: Clústeres invitados en una red virtual
description: Las máquinas virtuales conectadas a una red virtual solo se pueden usar las direcciones IP que asignó controladora de red para comunicarse en la red.  Tecnologías de agrupación en clústeres que requieren una dirección IP flotante, como clústeres de conmutación por error de Microsoft, requieren algunos pasos adicionales para funcionar correctamente.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: 97c20fd07d06b609686daf4d6308a9f248873036
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446342"
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clústeres invitados en una red virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Las máquinas virtuales conectadas a una red virtual solo se pueden usar las direcciones IP que asignó controladora de red para comunicarse en la red.  Tecnologías de agrupación en clústeres que requieren una dirección IP flotante, como clústeres de conmutación por error de Microsoft, requieren algunos pasos adicionales para funcionar correctamente.

El método para hacer que la dirección IP flotante accesible consiste en usar un equilibrador de carga de Software \(SLB\) IP virtual \(VIP\).  El equilibrador de carga de software debe configurarse con un sondeo de estado en un puerto en esa dirección IP para que el SLB dirige el tráfico a la máquina que actualmente tiene esa dirección IP.


## <a name="example-load-balancer-configuration"></a>Por ejemplo: Configuración del equilibrador de carga

En este ejemplo se da por supuesto que ya ha creado las máquinas virtuales que se convertirán en nodos de clúster y les asociadas a una red Virtual.  Para obtener instrucciones, consulte [crear una máquina virtual y conectarse a una red Virtual del inquilino o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

En este ejemplo se crea una dirección IP virtual (192.168.2.100) para representar la dirección IP flotante del clúster y configurar un sondeo de estado para supervisar el puerto TCP 59999 para determinar qué nodo está activa.

1. Seleccione a la dirección VIP.<p>Prepare asignando una dirección IP de VIP, que puede ser cualquier dirección reservada o no usado en la misma subred que los nodos del clúster.  La dirección VIP debe coincidir con la dirección del clúster de flotante.

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

3. Crear un front\-dirección IP final.

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

4. Crear una copia\-terminar el grupo que contiene los nodos del clúster.

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. Agregue un sondeo para detectar qué nodo de clúster está activa actualmente en la dirección de flotante. 

   >[!NOTE]
   >La consulta de sondeo en la dirección permanente de la máquina virtual en el puerto que se definen a continuación.  El puerto solo debe responder en el nodo activo. 

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

6. Agregue las reglas para el puerto TCP 1433 de equilibrio de carga.<p>Puede modificar el protocolo y puerto según sea necesario.  También puede repetir este paso varias veces para puertos adicionales y protcols en esta dirección VIP.  Es importante que EnableFloatingIP está establecido en $true porque esto indica que el equilibrador de carga para enviar el paquete en el nodo con la dirección VIP original en su lugar.

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

8. Agregue los nodos del clúster para el grupo de back-end.<p>Puede agregar tantos nodos al grupo como sea necesario para el clúster.

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

   Una vez que ha creado el equilibrador de carga y agregar las interfaces de red para el grupo de back-end, está listo para configurar el clúster.  

9. (Opcional) Si utilizas a Microsoft Failover Cluster, continúe con el ejemplo siguiente. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Ejemplo 2: Configurar un clúster de conmutación por error de Microsoft

Puede usar los pasos siguientes para configurar un clúster de conmutación por error.

1. Instalar y configurar las propiedades de un clúster de conmutación por error.

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = “192.168.2.100” 

   $nodes = @("DB1", "DB2")
   ```

2. Cree el clúster en un nodo.

   ```PowerShell
   New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]
   ```

3. Detener el recurso de clúster.

   ```PowerShell
   Stop-ClusterResource "Cluster Name" 
   ```

4. Establecer el clúster puerto IP y el sondeo.<p>La dirección IP debe coincidir con la dirección ip de front-end usada en el ejemplo anterior, y el puerto de sondeo debe coincidir con el puerto de sondeo en el ejemplo anterior.

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. Iniciar los recursos de clúster.

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. Agregue los nodos restantes.

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**El clúster está activo.** _ Se dirige el tráfico que va a la dirección VIP en el puerto especificado en el nodo activo.

---