---
title: Invitado clústeres en una red Virtual
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 5cab7e7c0ca0af848b4b58362388701cc4357860
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="guest-clustering-in-a-virtual-network"></a>Invitado clústeres en una red Virtual

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Máquinas virtuales que están conectados a una red virtual solo tienen permiso para usar las direcciones IP que se le asigna el controlador de red con el fin de comunicar en la red.  Esto significa clústeres tecnologías que requieren una dirección IP flotante, como clústeres de conmutación por error de Microsoft, requieren algunos pasos adicionales para funcionar correctamente.

El método para hacer que la dirección IP flotante accesible es usar un \(SLB\) equilibrado de carga de Software virtual \(VIP\) IP.  El equilibrado de carga de software debe configurarse con un sondeo del estado en un puerto en esa dirección IP para que SLB dirige el tráfico a la máquina que actualmente tiene esa dirección IP.

## <a name="example-load-balancer-configuration"></a>Ejemplo: Configuración de equilibrado de carga

En este ejemplo se da por hecho que ya has creado las máquinas virtuales que se convertirá en nodos del clúster y les asociadas a una red Virtual.  Para obtener instrucciones, consulta [crear una máquina virtual y conectarse a una red Virtual de inquilino o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

En este ejemplo se crea una dirección IP virtual (192.168.2.100) para representar la dirección IP flotante del clúster y configurar un sondeo del estado para controlar el puerto TCP 59999 para determinar qué nodo está activa.

### <a name="step-1-select-the-vip"></a>Paso 1: Selecciona la dirección VIP
Preparar asignando una dirección IP VIP.  Esta dirección puede ser cualquier dirección reservado o no usado en la misma subred que los nodos del clúster.  La dirección VIP debe coincidir con la dirección flotante del clúster.

    $VIP = "192.168.2.100"
    $subnet = "Subnet2"
    $VirtualNetwork = "MyNetwork"
    $ResourceId = "MyNetwork_InternalVIP"

### <a name="step-2-create-the-load-balancer-properties-object"></a>Paso 2: Crear el objeto de propiedades de carga equilibrado

Puedes usar el siguiente comando de ejemplo para crear el objeto de propiedades de carga equilibrado.

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

### <a name="step-3-create-a-front-end-ip-address"></a>Paso 3: Crear una dirección IP de front\ final

Puedes usar el siguiente comando de ejemplo para crear una dirección IP de front\-end.

    $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEnd.resourceId = "Frontend1"
    $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
    $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
    $FrontEnd.properties.privateIPAddress = $VIP
    $FrontEnd.properties.privateIPAllocationMethod = "Static"

### <a name="step-4-create-a-back-end-pool-to-contain-the-cluster-nodes"></a>Paso 4: Crear un grupo de back\-end para contener los nodos del clúster

Puedes usar el siguiente comando de ejemplo para crear un grupo de back\ final

    $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
    $BackEnd.resourceId = "Backend1"
    $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
    $LoadBalancerProperties.backendAddressPools += $BackEnd

### <a name="step-5-add-a-probe"></a>Paso 5: Agregar un sondeo
El sondeo es necesario para detectar qué nodo del clúster está activa actualmente en la dirección flotante.

>[!NOTE]
>La consulta de sondeo en la dirección permanente de la máquina virtual en el puerto que se define a continuación.  Solo debes responder el puerto en el nodo activo. 

    $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties.protocol = "TCP"
    $lbprobe.properties.port = "59999"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11

### <a name="step-5-add-the-load-balancing-rules"></a>Paso 5: Agregar las reglas de equilibrio de carga
Este paso crea una regla para el puerto TCP 1433 equilibrio de carga.  Puedes modificar el protocolo y el puerto según sea necesario.  También puede repetir este paso varias veces para puertos adicionales y los protocolos que en este VIP.  Es importante que EnableFloatingIP está establecido en $true porque esto indica la equilibrado de carga para enviar el paquete para el nodo con la dirección VIP original en su lugar.

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

### <a name="step-5-create-the-load-balancer-in-network-controller"></a>Paso 5: Crear el equilibrado de carga en el controlador de red

Puedes usar el siguiente comando de ejemplo para crear el equilibrado de carga.

    $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force

### <a name="step-6-add-the-cluster-nodes-to-the-backend-pool"></a>Paso 6: Agregar los nodos del clúster al grupo de back-end

Este ejemplo muestra cómo se agregan dos miembros del grupo, pero puedes agregar tantas nodos al grupo como sea necesario para el clúster.

    # Cluster Node 1

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

Una vez que hayas creado el equilibrado de carga y agregado las interfaces de red al grupo de back-end, estás listo para configurar el clúster.  Si estás usando un clúster Failover de Microsoft puede continuar con el siguiente ejemplo. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Ejemplo 2: Configurar un clúster de conmutación por error de Microsoft

Puedes usar los siguientes pasos para configurar un clúster de conmutación por error.

### <a name="step-1-install-failover-clustering"></a>Paso 1: Instalar clústeres de conmutación por error

Puedes usar los siguientes comandos de ejemplo para instalar y configurar las propiedades de un clúster de conmutación por error.

    add-windowsfeature failover-clustering -IncludeManagementTools
    Import-module failoverclusters

    $ClusterName = "MyCluster"
   
    $ClusterNetworkName = "Cluster Network 1"
    $IPResourceName =  
    $ILBIP = “192.168.2.100” 

    $nodes = @("DB1", "DB2")

### <a name="step-2-create-the-cluster-on-one-node"></a>Paso 2: Crear el clúster en un nodo

Puedes usar el siguiente comando de ejemplo para crear el clúster en un nodo.

    New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]

### <a name="step-3-stop-the-cluster-resource"></a>Paso 3: Detener el recurso de clúster

Puedes usar el siguiente comando de ejemplo para detener el recurso de clúster.

    Stop-ClusterResource "Cluster Name" 

### <a name="step-4-set-the-cluster-ip-and-probe-port"></a>Paso 4: Establecer el clúster puertos IP y sondeo
La dirección IP debe coincidir con la dirección ip front-end utilizada en el ejemplo anterior, y el puerto de sondeo debe coincidir con el puerto de sondeo en el ejemplo anterior.

    Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

### <a name="step-5-start-the-cluster-resources"></a>Paso 5: Iniciar los recursos de clúster

Puedes usar el siguiente comando de ejemplo para empezar a los recursos del clúster.

    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 

### <a name="step-6-add-the-remaining-nodes"></a>Paso 6: Agregar el resto de los nodos

Puedes usar el siguiente comando de ejemplo para agregar nodos del clúster.

    Add-ClusterNode $nodes[1]

Una vez finalizado el último paso, el clúster está activo. Se dirige el tráfico que se va a la dirección VIP en el puerto especificado en el nodo activo.