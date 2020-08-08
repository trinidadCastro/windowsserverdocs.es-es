---
title: Configuración del equilibrador de carga de software para el equilibrio de carga y la traducción de direcciones de red (NAT)
description: Este tema forma parte de la guía de redes definidas por software sobre cómo administrar cargas de trabajo de inquilinos y redes virtuales en Windows Server 2016.
manager: grcusanz
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: e7488c546753594f61e034b271fcaddfd52a0233
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954051"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configuración del equilibrador de carga de software para el equilibrio de carga y la traducción de direcciones de red (NAT)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo usar el SLB del equilibrador de carga de software de redes definidas por software \( Sdn \) \( \) para proporcionar \( NAT de traducción de direcciones de red salientes \) , NAT de entrada o equilibrio de carga entre varias instancias de una aplicación.

## <a name="software-load-balancer-overview"></a>Información general de Load Balancer de software

El software de SDN Load Balancer \( SLB \) ofrece alta disponibilidad y rendimiento de red a las aplicaciones. Es un \( equilibrador de carga TCP de nivel 4, UDP \) que distribuye el tráfico entrante entre las instancias de servicio en buen estado en Cloud Services o máquinas virtuales definidas en un conjunto de equilibrador de carga.

Configure SLB para hacer lo siguiente:

- Equilibre la carga del tráfico entrante externo a una red virtual a las máquinas virtuales de virtual machines \( \) , también denominado equilibrio de carga VIP público.
- Equilibre la carga del tráfico entrante entre máquinas virtuales de una red virtual, entre máquinas virtuales de Cloud Services o entre equipos locales y máquinas virtuales en una red virtual entre locales.
- Reenvíe el tráfico de red de VM desde la red virtual a destinos externos mediante la traducción de direcciones de red (NAT), también denominada NAT de salida.
- Reenviar el tráfico externo a una máquina virtual específica, también denominada NAT entrante.




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>Ejemplo: creación de una VIP pública para el equilibrio de carga de un grupo de dos máquinas virtuales en una red virtual

En este ejemplo, creará un objeto de equilibrador de carga con una VIP pública y dos máquinas virtuales como miembros del grupo para atender las solicitudes a la dirección VIP. Este código de ejemplo también agrega un sondeo de Estado HTTP para detectar si uno de los miembros del grupo deja de responder.

1. Prepare el objeto de equilibrador de carga.

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. Asigne una dirección IP de front-end, que normalmente se conoce como IP virtual (VIP).<p>La VIP debe ser de una IP sin usar en uno de los grupos de direcciones IP de red lógica que se proporcionan al administrador de equilibrador de carga.

   ```PowerShell
    $VIPIP = "10.127.134.5"
    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException

    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"

    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig
   ```

3. Asigne un grupo de direcciones de back-end que contenga las direcciones IP dinámicas (DIP) que componen los miembros del conjunto de máquinas virtuales con equilibrio de carga.

   ```PowerShell
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. Defina un sondeo de estado que use el equilibrador de carga para determinar el estado de mantenimiento de los miembros del grupo de back-end.<p>En este ejemplo, se define un sondeo HTTP que realiza consultas al RequestPath de "/health.htm".  La consulta se ejecuta cada 5 segundos, según se especifica en la propiedad IntervalInSeconds.<p>El sondeo de Estado debe recibir un código de respuesta HTTP de 200 para 11 consultas consecutivas para que el sondeo tenga en cuenta que la dirección IP de back-end es correcta. Si la dirección IP de back-end no es correcta, no recibe tráfico del equilibrador de carga.

   >[!IMPORTANT]
   >No bloquee el tráfico hacia o desde la primera dirección IP de la subred para las listas de Access Control (ACL) que se aplican a la dirección IP de back-end, ya que es el punto de origen de los sondeos.

   Use el ejemplo siguiente para definir un sondeo de estado.

   ```PowerShell
    $Probe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $Probe.ResourceId = "Probe1"
    $Probe.ResourceRef = "/loadBalancers/$LBResourceId/Probes/$($Probe.ResourceId)"

    $Probe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties
    $Probe.properties.Protocol = "HTTP"
    $Probe.properties.Port = "80"
    $Probe.properties.RequestPath = "/health.htm"
    $Probe.properties.IntervalInSeconds = 5
    $Probe.properties.NumberOfProbes = 11

    $LoadBalancerProperties.Probes += $Probe
   ```

5. Defina una regla de equilibrio de carga para enviar el tráfico que llega a la dirección IP de front-end a la IP de back-end.  En este ejemplo, el grupo de back-end recibe el tráfico TCP al puerto 80.<p>Use el ejemplo siguiente para definir una regla de equilibrio de carga:

   ```PowerShell
   $Rule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $Rule.ResourceId = "webserver1"

   $Rule.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $Rule.Properties.FrontEndIPConfigurations += $FrontEndIPConfig
   $Rule.Properties.backendaddresspool = $BackEndAddressPool
   $Rule.Properties.protocol = "TCP"
   $Rule.Properties.FrontEndPort = 80
   $Rule.Properties.BackEndPort = 80
   $Rule.Properties.IdleTimeoutInMinutes = 4
   $Rule.Properties.Probe = $Probe

   $LoadBalancerProperties.loadbalancingRules += $Rule
   ```

6. Agregue la configuración del equilibrador de carga a la controladora de red.<p>Use el ejemplo siguiente para agregar la configuración del equilibrador de carga a la controladora de red:

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. Siga el ejemplo siguiente para agregar las interfaces de red a este grupo de back-end.


## <a name="example-use-slb-for-outbound-nat"></a>Ejemplo: uso de SLB para NAT de salida

En este ejemplo, se configura SLB con un grupo de back-end para proporcionar la funcionalidad NAT saliente para una máquina virtual en el espacio de direcciones privadas de una red virtual para llegar a la salida a Internet.

1. Cree las propiedades del equilibrador de carga, la dirección IP de front-end y el grupo de back-end.

   ```PowerShell
    import-module NetworkController
    $URI = "https://sdn.contoso.com"

    $LBResourceId = "OutboundNATMMembers"
    $VIPIP = "10.127.134.6"

    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"

    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig

    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"
    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

2. Defina la regla NAT de salida.

   ```PowerShell
    $OutboundNAT = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRule
    $OutboundNAT.ResourceId = "onat1"

    $OutboundNAT.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRuleProperties
    $OutboundNAT.properties.frontendipconfigurations += $FrontEndIPConfig
    $OutboundNAT.properties.backendaddresspool = $BackEndAddressPool
    $OutboundNAT.properties.protocol = "ALL"

    $LoadBalancerProperties.OutboundNatRules += $OutboundNAT
   ```

3. Agregue el objeto de equilibrador de carga en la controladora de red.

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. Siga el siguiente ejemplo para agregar las interfaces de red a las que desea proporcionar acceso a Internet.

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>Ejemplo: agregar interfaces de red al grupo de back-end
En este ejemplo, agregará interfaces de red al grupo de back-end.  Debe repetir este paso para cada interfaz de red que pueda procesar las solicitudes realizadas a la VIP.

También puede repetir este proceso en una sola interfaz de red para agregarlo a varios objetos de equilibrador de carga. Por ejemplo, si tiene un objeto de equilibrador de carga para una VIP de servidor Web y un objeto de equilibrador de carga independiente para proporcionar NAT de salida.

1. Obtenga el objeto de equilibrador de carga que contiene el grupo de back-end para agregar una interfaz de red.

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
   ```

2. Obtenga la interfaz de red y agregue el grupo backendaddress a la matriz loadbalancerbackendaddresspools.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```

3. Coloque la interfaz de red para aplicar el cambio.

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ```


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>Ejemplo: usar el Load Balancer de software para reenviar el tráfico
Si necesita asignar una dirección IP virtual a una sola interfaz de red en una red virtual sin definir puertos individuales, puede crear una regla de reenvío L3.  Esta regla reenvía todo el tráfico hacia y desde la máquina virtual a través de la VIP asignada contenida en un objeto PublicIPAddress.

Si ha definido la dirección VIP y la DIP como la misma subred, es equivalente a realizar el reenvío L3 sin NAT.

>[!NOTE]
>Este proceso no requiere la creación de un objeto de equilibrador de carga.  Asignar PublicIPAddress a la interfaz de red es suficiente información para que el Load Balancer de software realice su configuración.


1. Cree un objeto de dirección IP pública que contenga la dirección VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Asigne el PublicIPAddress a una interfaz de red.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>Ejemplo: usar el Load Balancer de software para reenviar el tráfico con una VIP asignada dinámicamente
En este ejemplo se repite la misma acción que en el ejemplo anterior, pero se asigna automáticamente la dirección VIP del grupo de direcciones VIP disponibles en el equilibrador de carga en lugar de especificar una dirección IP específica.

1. Cree un objeto de dirección IP pública que contenga la dirección VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Consulte el recurso PublicIPAddress para determinar qué dirección IP se asignó.

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   La propiedad IpAddress contiene la dirección asignada.  El resultado será similar al siguiente:
   ```
    Counters                 : {}
    ConfigurationState       :
    IpAddress                : 10.127.134.2
    PublicIPAddressVersion   : IPv4
    PublicIPAllocationMethod : Dynamic
    IdleTimeoutInMinutes     : 4
    DnsSettings              :
    ProvisioningState        : Succeeded
    IpConfiguration          :
    PreviousIpConfiguration  :
   ```

3. Asigne el PublicIPAddress a una interfaz de red.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
   ## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>Ejemplo: quitar una dirección PublicIP que se usa para reenviar el tráfico y devolverla al grupo VIP
   En este ejemplo se quita el recurso PublicIPAddress creado por los ejemplos anteriores.  Una vez que se quita el PublicIPAddress, la referencia a PublicIPAddress se quitará automáticamente de la interfaz de red, el tráfico dejará de reenviarse y la dirección IP se devolverá al grupo de VIP público para reutilizarlo.

4. Quitar la PublicIP

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


