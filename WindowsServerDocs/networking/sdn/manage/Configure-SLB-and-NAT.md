---
title: Configurar el equilibrado de carga de Software de equilibrio de carga y de red (NAT) de traducción de direcciones
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 7f0393db564061caa0bc8f18b1d623f24749b46c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurar el equilibrado de carga de Software de equilibrio de carga y de red (NAT) de traducción de direcciones

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo usar el equilibrado de carga de software \(SDN\) definido redes Software \(SLB\) para proporcionar la traducción de direcciones de red saliente NAT, NAT entrante, o para cargar el equilibrio entre varias instancias de una aplicación.

Este tema contiene las siguientes secciones.

- [Información general de equilibrado de carga de software](#bkmk_slbover)
- [Ejemplo: Crear a una VIP pública de un grupo de máquinas dos virtuales en una red virtual de equilibrio de carga](#bkmk_publicvip)
- [Ejemplo: Usar SLB para NAT saliente](#bkmk_obnat)
- [Ejemplo: Agregar interfaces de red al grupo de fondo](#bkmk_backend)
- [Ejemplo: Usar el equilibrado de carga de Software para reenviar el tráfico](#bkmk_forward)

## <a name="bkmk_slbover"></a>Información general de equilibrado de carga de software

La \(SLB\) SDN Software carga equilibrado proporciona alto rendimiento de red y la disponibilidad de las aplicaciones. Es un nivel 4 \ (TCP, UDP\) cargar equilibrado que distribuye el tráfico entrante entre instancias de servicio correcto en servicios en la nube o máquinas virtuales definidos en un conjunto de carga equilibrado.

Puedes configurar SLB para hacer lo siguiente.

* Carga tráfico entrante de equilibrio externo a una red virtual para \(VMs\) máquinas virtuales. Esto se denomina equilibrio de carga de VIP pública.
* Carga tráfico entrante de equilibrio entre máquinas virtuales en una red virtual, entre las máquinas virtuales en servicios en la nube o entre los equipos locales y máquinas virtuales en una red virtual de entre local. 
* Reenviar el tráfico de red VM desde la red virtual a destinos externos con la traducción de direcciones de red (NAT).  Esto se denomina NAT de salida.
* Reenviar tráfico externo a una máquina virtual específica.  Esto se denomina entrante de NAT.

>[!IMPORTANT]
>Un problema conocido impide que los objetos de equilibrado de carga en el módulo de PowerShell de NetworkController Windows funciona correctamente en Windows Server 2016 5. La solución alternativa es usar tablas dinámicas hash e Invoke-WebRequest en su lugar. Este método se muestra en los siguientes ejemplos.


## <a name="bkmk_publicvip"></a>Ejemplo: Crear a una VIP pública de un grupo de máquinas dos virtuales en una red virtual de equilibrio de carga

Puedes usar este ejemplo para crear un objeto de carga equilibrado con VIP pública y dos máquinas virtuales como miembros del grupo para atender solicitudes a la dirección VIP.  Este código de ejemplo también agrega un sondeo del estado HTTP para detectar si uno de los miembros del grupo se convierte en no responde.

###<a name="step-1-prepare-the-load-balancer-object"></a>Paso 1: Preparar el objeto de carga equilibrado
Puedes usar el siguiente ejemplo para preparar el objeto de carga equilibrado.

    $lbresourceId = "LB2"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

###<a name="step-2-assign-a-front-end-ip"></a>Paso 2: Asignar un front-end IP
La dirección IP front-end se conoce comúnmente como IP Virtual (VIP).  La dirección VIP debe tomarse desde una dirección IP no utilizada en uno de la red lógica grupo IP proporcionado anteriormente en el Administrador de equilibrado de carga.

Puedes usar el siguiente ejemplo para asignar una dirección IP front-end.

    $vipip = "10.127.132.5"
    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

###<a name="step-3-allocate-a-backend-address-pool"></a>Paso 3: Asignar un conjunto de direcciones de back-end
El conjunto de direcciones de back-end contiene las direcciones IP dinámica (DIP) que conforman los miembros del conjunto de equilibrio de carga de máquinas virtuales. En este paso solo se asigne el grupo; las configuraciones de IP se agregan en un paso posterior.

Puedes usar el siguiente ejemplo para asignar un conjunto de direcciones de back-end.
 
    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-4-define-a-health-probe"></a>Paso 4: Define un sondeo del estado
Comprobaciones de estado se usan por el equilibrado de carga para determinar el estado de los miembros del grupo de back-end. Con este ejemplo, definen un sondeo HTTP que consulta a la RequestPath de "/ health.htm".  La consulta se realiza cada 5 segundos, como se especifica por la propiedad IntervalInSeconds.

El sondeo del estado debe recibir un código de respuesta HTTP 200 para realizar consultas consecutivas 11 para el sondeo a tener en cuenta la dirección IP de back-end para estar en buen estado. Si la dirección IP de back-end no es correcto, el equilibrado de carga no enviará el tráfico a la dirección IP.

>[!Note]
>Es importante que las listas de Control de acceso que se aplican a la dirección IP de back-end no bloquee el tráfico a o desde la primera dirección IP de la subred, ya que es el punto de origen para los sondeos.

Puedes usar el siguiente ejemplo para definir un sondeo del estado.
 
    $lbprobe = @{}
    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$lbresourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties = @{}
    $lbprobe.properties.protocol = "HTTP"
    $lbprobe.properties.port = "80"
    $lbprobe.properties.RequestPath = "/health.htm"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11
    $lbproperties.probes += $lbprobe

###<a name="step-5-define-a-load-balancing-rule"></a>Paso 5: Definir una regla de equilibrio de carga
Esta regla de equilibrio de carga define cómo está el tráfico que llega a la dirección IP front-end que se enviará a la dirección IP de back-end.  En este ejemplo, el tráfico TCP con el puerto 80 se envía al grupo de back-end.

Puedes usar el siguiente ejemplo para definir una regla de equilibrio de carga.

    $lbrule = @{}
    $lbrule.ResourceId = "webserver1"
    $lbrule.properties = @{}
    $lbrule.properties.FrontEndIPConfigurations = @()
    $lbrule.properties.FrontEndIPConfigurations += $fe
    $lbrule.properties.backendaddresspool = $backend 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = 80
    $lbrule.properties.Probe = $lbprobe
    $lbproperties.loadbalancingRules += $lbrule

###<a name="step-6-add-the-load-balancer-configuration-to-network-controller"></a>Paso 6: Agregar la configuración de carga equilibrado al controlador de red
Hasta ahora en este ejemplo, son todos los objetos creados en la memoria de la sesión de Windows PowerShell. Este paso agrega los objetos al controlador de red.

Puedes usar el siguiente ejemplo para agregar la configuración de carga equilibrado al controlador de red.

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

Después de este paso, deberás seguir el ejemplo siguiente para agregar las interfaces de red a este grupo de back-end.

## <a name="bkmk_obnat"></a>Ejemplo: Usar SLB para NAT saliente

Puedes usar este ejemplo para configurar SLB con un grupo de back-end para proporcionar funcionalidad NAT saliente para que una máquina virtual en el espacio de direcciones privadas de una red virtual llegar a saliente a internet.

###<a name="step-1-create-the-loadbalancer-properties-front-end-ip-and-backend-pool"></a>Paso 1: Crear las propiedades de dirección, front-end IP y grupo de back-end.
Puedes usar el siguiente ejemplo para crear las propiedades de dirección, front-end IP y grupo de back-end.

    $lbresourceId = "OutboundNATMembers"
    $vipip = "10.127.132.7"

    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-2-define-the-outbound-nat-rule"></a>Paso 2: Definir la regla NAT saliente
Puedes usar el siguiente ejemplo para definir la regla NAT saliente. 

    $onat = @{}
    $onat.ResourceId = "onat1"
    $onat.properties = @{}
    $onat.properties.frontendipconfigurations = @()
    $onat.properties.frontendipconfigurations += $fe
    $onat.properties.backendaddresspool = $backend
    $onat.properties.protocol = "ALL"
    $lbproperties.OutboundNatRules += $onat

###<a name="step-3-add-the-load-balancer-object-in-network-controller"></a>Paso 3: Agregar el objeto de equilibrado de carga en el controlador de red
Puedes usar el siguiente ejemplo para agregar el objeto de equilibrado de carga en el controlador de red.

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

En el siguiente paso, puedes agregar las interfaces de red al que quieres proporcionar acceso a internet.

## <a name="bkmk_backend"></a>Ejemplo: Agregar interfaces de red al grupo de fondo
Puedes usar este ejemplo para agregar las interfaces de red al grupo de back-end.

Debe repetir este paso para cada interfaz de red que puede procesar las solicitudes que se realizan en la dirección VIP. También puedes repetir este proceso en una única interfaz de red para agregarla a varios objetos de equilibrado de carga. Por ejemplo, si tienes un objeto de equilibrado de carga para un servidor Web VIP y un objeto de equilibrado de carga independientes para proporcionar NAT de salida.
    
### <a name="step-1-get-the-load-balancer-object-containing-the-back-end-pool-to-which-you-will-add-a-network-interface"></a>Paso 1: Obtener el objeto de equilibrado de carga que contiene el grupo de back-end al que agregar una interfaz de red
Puedes usar el siguiente ejemplo para recuperar el objeto de carga equilibrado.

    $lbresourceid = "LB2"
    $lb = (Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Get" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -DisableKeepAlive -UseBasicParsing).content | convertfrom-json 

### <a name="step-2-get-the-network-interface-and-add-the-backendaddress-pool-to-the-loadbalancerbackendaddresspools-array"></a>Paso 2: Obtener la interfaz de red y agrega el grupo de backendaddress a la matriz de loadbalancerbackendaddresspools.
Para obtener la interfaz de red y agrega el grupo de backendaddress a la matriz de loadbalancerbackendaddresspools puedes usar el siguiente ejemplo.

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    
### <a name="step-3-put-the-network-interface-to-apply-the-change"></a>Paso 3: Coloca la interfaz de red para aplicar el cambio
Puedes usar el siguiente ejemplo de poner la interfaz de red para aplicar el cambio.

    new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force
 
## <a name="bkmk_forward"></a>Ejemplo: Usar el equilibrado de carga de Software para reenviar el tráfico
Si es necesario asignar una dirección IP Virtual a una única interfaz de red en una red virtual sin definir puertos individuales, puedes crear una regla de reenvío L3.  Esta regla reenvía todo el tráfico de la máquina virtual a través de la dirección VIP asignada, que debe estar incluida en un objeto PublicIPAddress.

Si la VIP y DIP se definen como la misma subred, a continuación, esto equivale a realizar L3 reenvío sin NAT.

>[!NOTE]
>Este proceso no requiere que crear un objeto de carga equilibrado.  Asignar la PublicIPAddress a la interfaz de red es suficiente información para que el equilibrado de carga de Software para llevar a cabo su configuración.

###<a name="step-1-create-a-public-ip-object-to-contain-the-vip"></a>Paso 1: Crear un objeto IP público para que contenga la dirección VIP
Puedes usar el siguiente ejemplo para crear un objeto IP público.

    $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
    $publicIPProperties.ipaddress = "10.127.132.6"
    $publicIPProperties.PublicIPAllocationMethod = "static"
    $publicIPProperties.IdleTimeoutInMinutes = 4
    $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri

####<a name="step-2-assign-the-publicipaddress-to-a-network-interface"></a>Paso 2: Asignar el PublicIPAddress a una interfaz de red
Puedes usar el siguiente ejemplo para asignar el PublicIPAddress a una interfaz de red.

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
    New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties



 