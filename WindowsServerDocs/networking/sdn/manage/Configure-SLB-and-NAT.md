---
title: Configuración del equilibrador de carga de software para el equilibrio de carga y la traducción de direcciones de red (NAT)
description: En este tema forma parte de la Guía de redes definidas por Software acerca de cómo administrar las cargas de trabajo de inquilinos y redes virtuales en Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 55847bfbc0362887497514009f6efe1312d79906
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819356"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configuración del equilibrador de carga de software para el equilibrio de carga y la traducción de direcciones de red (NAT)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprender a usar las redes de definidas por Software \(SDN\) equilibrador de carga de software \(SLB\) para proporcionar la traducción de direcciones de red saliente \(NAT\), NAT de entrada o equilibrio de carga entre varias instancias de una aplicación.

## <a name="software-load-balancer-overview"></a>Información general del equilibrador de carga de software

El equilibrador de carga de Software de SDN \(SLB\) proporciona alta disponibilidad y rendimiento de red para sus aplicaciones. Es una capa 4 \(TCP, UDP\) equilibrador que distribuye el tráfico entrante entre instancias de servicio en buen estado en servicios en la nube o máquinas virtuales definidas en un conjunto de equilibrador de carga de carga.

Configurar el SLB para hacer lo siguiente:

- Equilibrar la carga entrante externo a una red virtual a las máquinas virtuales \(máquinas virtuales\), también denominada equilibrio de carga de VIP pública.
- Carga equilibrar el tráfico entrante entre las máquinas virtuales en una red virtual, entre las máquinas virtuales en servicios en la nube o entre equipos locales y máquinas virtuales en una red virtual entre locales. 
- Reenviar el tráfico de red de máquina virtual desde la red virtual a destinos externos mediante la traducción de direcciones de red (NAT), que también se denomina NAT de salida.
- Reenviar el tráfico externo a una máquina virtual específica, también se denomina NAT de entrada.




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>Por ejemplo: Crear a una VIP pública para un grupo de dos máquinas virtuales en una red virtual de equilibrio de carga

En este ejemplo, cree un objeto de equilibrador de carga con una VIP pública y dos máquinas virtuales como miembros del grupo para atender las solicitudes a la dirección VIP. Este código de ejemplo también agrega un sondeo de estado HTTP para detectar si uno de los miembros del grupo deja de responder.

1. Prepare el objeto de equilibrador de carga.

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. Asigne una dirección IP front-end, que se conoce comúnmente como una IP Virtual (VIP).<p>Debe ser la dirección VIP desde una dirección IP sin usar en uno de los grupos de IP de red lógica proporcionados para el administrador del equilibrador de carga. 

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

3. Asignar un grupo de direcciones de back-end, que contiene las direcciones IP dinámicas (DIP) que constituyen los miembros del conjunto con equilibrio de carga de máquinas virtuales. 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. Definir un sondeo de estado que el equilibrador de carga se usa para determinar el estado de mantenimiento de los miembros del grupo de back-end.<p>En este ejemplo, definir un sondeo HTTP que consulta a RequestPath de "/ health.htm."  La consulta se ejecuta cada 5 segundos, según lo especificado por la propiedad IntervalInSeconds.<p>El sondeo de estado debe recibir un código de respuesta HTTP 200 para 11 consultas consecutivas para el sondeo a tener en cuenta la dirección IP de back-end sea correcto. Si la dirección IP de back-end no es correcta, no recibe tráfico del equilibrador de carga.

   >[!IMPORTANT]
   >No bloquee el tráfico hacia o desde la primera dirección IP en la subred de cualquier Control de listas de acceso (ACL) que se aplican a la dirección IP de back-end, ya que es el punto de origen para los sondeos.

   Use el siguiente ejemplo para definir un sondeo de estado.

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

5.  Definir una regla para enviar el tráfico que llega a la dirección IP de front-end a la dirección IP de back-end de equilibrio de carga.  En este ejemplo, el grupo de back-end recibe el tráfico TCP al puerto 80.<p>Use el siguiente ejemplo para definir una regla de equilibrio de carga:

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


## <a name="example-use-slb-for-outbound-nat"></a>Por ejemplo: Usar SLB para NAT saliente

En este ejemplo, configura el SLB con un grupo de back-end para proporcionar capacidad saliente de NAT para que una máquina virtual en el espacio de direcciones privadas de una red virtual llegar a saliente a internet. 

1. Crear las propiedades del equilibrador de carga, IP de front-end y grupo back-end.

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

2. Definir la regla NAT saliente.

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

4. Siga el ejemplo siguiente para agregar las interfaces de red a la que desea proporcionar acceso a internet.

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>Por ejemplo: Agregar interfaces de red para el grupo de back-end
En este ejemplo, agregar interfaces de red para el grupo de back-end.  Debe repetir este paso para cada interfaz de red que puede procesar las solicitudes realizadas a la dirección VIP. 

También puede repetir este proceso en una única interfaz de red para agregarlo a varios objetos de equilibrador de carga. Por ejemplo, si tiene un objeto de equilibrador de carga para una VIP de servidor web y un objeto de equilibrador de carga independiente para proporcionar NAT de salida.
    
1. Obtenga el objeto de equilibrador de carga que contiene el grupo de back-end para agregar una interfaz de red.

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
  ```

2. Obtenga la interfaz de red y agregue el grupo de backendaddress a la matriz loadbalancerbackendaddresspools.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. Colocar la interfaz de red para aplicar el cambio. 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>Por ejemplo: Utilice el equilibrador de carga de Software para reenviar el tráfico
Si necesita asignar una dirección IP Virtual a una sola interfaz de red en una red virtual sin definir puertos individuales, puede crear una regla de reenvío L3.  Esta regla reenvía todo el tráfico hacia y desde la máquina virtual a través de la VIP asignada contenida en un objeto de PublicIPAddress.

Si se ha definido la VIP y DIP como la misma subred, a continuación, esto es equivalente a la realización de reenvío L3 sin NAT.

>[!NOTE]
>Este proceso no requiere crear un objeto de equilibrador de carga.  Asignación de PublicIPAddress a la interfaz de red es suficiente información para el equilibrador de carga de Software realizar su configuración.


1. Crear un objeto IP pública para que contenga a la dirección VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Asignar PublicIPAddress a una interfaz de red.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>Por ejemplo: Utilice el equilibrador de carga de Software para reenviar el tráfico con una dirección VIP asignada dinámicamente
En este ejemplo se repite la misma acción que el ejemplo anterior, pero lo asigna automáticamente la dirección VIP del grupo de direcciones VIP disponible en el equilibrador de carga en lugar de especificar una dirección IP específica. 

1. Crear un objeto IP pública para que contenga a la dirección VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Consultar el recurso de PublicIPAddress para determinar qué dirección IP se asignó.

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   La propiedad IpAddress contiene la dirección asignada.  El resultado tendrá un aspecto similar al siguiente:
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
 
1. Asignar PublicIPAddress a una interfaz de red.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>Por ejemplo: Quitar una dirección PublicIP que se utiliza para reenviar el tráfico y devolverlo al grupo de VIP
En este ejemplo se quita el recurso de PublicIPAddress creados por los ejemplos anteriores.  Una vez que se quita el elemento PublicIPAddress, automáticamente se quitará la referencia a PublicIPAddress de la interfaz de red, detendrá el tráfico que se reenvían y se devolverá la dirección IP para el grupo de VIP públicas para su reutilización.  

1. Quitar la PublicIP

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 