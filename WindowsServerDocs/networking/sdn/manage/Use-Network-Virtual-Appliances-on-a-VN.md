---
title: Usar dispositivos de red Virtual en una red Virtual
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: db46189931263d230f013431f319eb2497589dee
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Usar dispositivos de red Virtual en una red Virtual

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprender a implementar los dispositivos de red virtual en el inquilino de redes virtuales.

Puedes agregar dispositivos de red virtual a las redes que realizan enrutamiento definida por el usuario y el puerto funciones de creación de reflejos.

Este tema contiene las siguientes secciones.

- [Tipos de dispositivos de red Virtual](#bkmk_types)
- [Implementación de un dispositivo de red Virtual](#bkmk_deploy)
- [Ejemplo: Definido por el usuario enrutamiento](#bkmk_routing)
- [Ejemplo: Creación de reflejo de puerto](#bkmk_port)

## <a name="bkmk_types"></a>Tipos de dispositivos de red Virtual

Hay dos tipos de accesorios virtuales que puedes usar en redes virtuales:

1. **Enrutamiento definido por el usuario**. Enrutamiento definida por el usuario reemplaza enrutadores distribuidos en la red virtual con las capacidades de enrutamiento del dispositivo virtual.  Con definida por el usuario enrutamiento, se usa el dispositivo virtual como un enrutador entre las subredes virtuales de la red virtual.
2. **Creación de reflejo de puerto**. Con la creación de reflejo de puerto, todo el tráfico de red que se entre o salga el puerto supervisado es duplicado y se envían a un dispositivo virtual para su análisis. 
## <a name="bkmk_deploy"></a>Implementación de un dispositivo de red Virtual

Para implementar un dispositivo virtual, debe primero, crea una máquina virtual (VM) que contiene el dispositivo y, a continuación, conectar la máquina virtual con las subredes de red virtual correspondiente.

>[!NOTE]
>Para obtener más información, consulta [crear una máquina virtual de inquilino y conectarse a una red Virtual de inquilino o VLAN](Create-a-Tenant-VM.md)

Algunos dispositivos requieren varios adaptadores de red virtual. Normalmente un adaptador de red está dedicado a la administración de dispositivo, mientras que otros adaptadores se utilizan para procesar el tráfico. 

Si el dispositivo requiere varios adaptadores de red, debe crear cada interfaz de red en el controlador de red. 

También debes asignar un identificador de la interfaz en cada equipo host para cada uno de los adaptadores adicionales que se encuentran en diferentes subredes virtuales.

Después de completar la implementación de dispositivos virtuales de red, puedes usar el dispositivo para el enrutamiento definida por el usuario, creación de reflejos de puerto o en ambos.

##<a name="bkmk_routing"></a>Ejemplo: Definido por el usuario enrutamiento

Para la mayoría de los entornos que solo tenga las rutas de sistema ya definidas por el enrutador distribuido de la red virtual. Sin embargo, es posible que debas crear una tabla de ruta y agregar una o varias rutas en casos específicos, como:

* Forzar túnel a Internet a través de la red local.
* Usar dispositivos virtuales en su entorno.

Para estos escenarios, debes crear una tabla de ruta y agregar rutas definida por el usuario a la tabla. Puedes tener varias tablas de ruta y la misma tabla de ruta puede asociarse a una o varias subredes. 

Solo se puede asociar a una tabla única ruta cada subred. Todas las máquinas virtuales en una subred usan la tabla de enrutamiento que está asociada a esa subred.

Las subredes dependen de rutas de sistema hasta que esté asociada a la subred una tabla de ruta. Cuando exista una asociación, el enrutamiento se realiza en función de más larga del prefijo coincidencia (LPM) entre las rutas de definida por el usuario y las rutas del sistema. 

Si hay más de una ruta con la misma coincidencia LPM, la ruta definida de usuario se selecciona primero - antes de la ruta del sistema. 

###<a name="step-1-create-the-route-table-properties"></a>Paso 1: Crear la ruta de propiedades de tabla

En esta tabla ruta contendrá todas las rutas definida por el usuario.  Rutas de sistema seguirá estando en vigor según las reglas definidas anteriormente.

Puedes usar los siguientes comandos de ejemplo para crear las propiedades de la tabla de ruta.

    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties

###<a name="step-2-add-a-route-to-the-route-table-properties"></a>Paso 2: Agregar una ruta a propiedades de la tabla de ruta

Esta ruta indica que debe obtener enviar todo el tráfico destinado a la subred 12.0.0.0/8 el equipo virtual en 192.168.1.10 enrutarse.  Es importante que el dispositivo tiene un adaptador de red virtual asociado a la red virtual con esa dirección IP asignada a una interfaz de red.

Puedes usar los siguientes comandos de ejemplo para agregar una ruta a propiedades de la tabla de ruta.

    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route

Puedes agregar rutas adicionales repetir este paso para cada ruta que quieras definir.
s
###<a name="step-3-add-the-route-table-to-network-controller"></a>Paso 3: Agregar la tabla de la ruta al controlador de red
Puedes usar los siguientes comandos de ejemplo para agregar la tabla de la ruta al controlador de red.

    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties

###<a name="step-4-apply-the-route-table-to-the-virtual-subnet"></a>Paso 4: La tabla de enrutamiento se aplican a la subred virtual
 
Cuando aplicas la tabla de enrutamiento en la subred virtual, la primera subred virtual en la red Tenant1_Vnet1 usa la tabla de enrutamiento. Puedes asignar la tabla de enrutamiento a todas las subredes de la red virtual como quieras.

Puedes usar los siguientes comandos de ejemplo para aplicar la tabla de enrutamiento en la subred virtual.

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid

Tan pronto como la tabla de enrutamiento se aplican a la red virtual, el tráfico se reenvía al dispositivo virtual. Debes configurar la tabla de enrutamiento en el dispositivo para reenviar el tráfico, de manera que sea apropiada para el entorno virtual.

##<a name="bkmk_port"></a>Ejemplo: Creación de reflejo de puerto

En este ejemplo te permite configurar el tráfico de MyVM_Ethernet1 para que se refleja el tráfico a Appliance_Ethernet1.

En este ejemplo se da por hecho que ya hayas implementado dos máquinas virtuales, uno que el dispositivo y otro que las máquinas virtuales para supervisar con la creación de reflejos.

Es importante que el dispositivo tiene una segunda interfaz de red para la administración de porque después de habilita la creación de reflejos como un destino en Appliance_Ethernet1, ya no recibirán tráfico destinado a la interfaz IP configurada.

###<a name="step-1-get-the-virtual-network-on-which-your-vms-are-located"></a>Paso 1: Obtener la red virtual en el que se encuentran tus máquinas virtuales
Puedes usar el siguiente comando de ejemplo para obtener la red virtual.

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"

###<a name="step-2-get-the-network-controller-network-interfaces-for-the-mirroring-source-and-destination"></a>Paso 2: Obtener las interfaces de red del controlador de red para la creación de reflejos origen y de destino
Puedes usar los siguientes comandos de ejemplo para obtener las interfaces de red de controlador de red para la creación de reflejos origen y destino.

    $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
    $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-3-create-a-serviceinsertionproperties-object-to-contain-the-port-mirroring-rules-and-the-element-which-represents-the-destination-interface"></a>Paso 3: Crear un objeto serviceinsertionproperties para contener el puerto de creación de reflejo de reglas y el elemento que representa la interfaz de destino
Puedes usar los siguientes comandos de ejemplo para crear un objeto de destino serviceinsertionproperties.

    $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
    $portMirror.Priority = 1

###<a name="step-4-create-a-serviceinsertionrules-object-to-contain-the-rules-that-must-be-matched-in-order-for-the-traffic-to-be-sent-to-the-appliance"></a>Paso 4: Crear un objeto de serviceinsertionrules para que contenga las reglas que deben cumplirse en orden para el tráfico que se envía al dispositivo

Las reglas definidas por debajo de la coincidencia de todo el tráfico, entrante y saliente, que representa un espejo tradicional.  Puedes ajustar estas reglas si estás interesado en un puerto específico u origen/destinos específicos de la creación de reflejos.

Puedes usar los siguientes comandos de ejemplo para crear un objeto serviceinsertionproperties.

    $portmirror.ServiceInsertionRules = [Microsoft.Windows.NetworkController.ServiceInsertionRule[]]::new(1)

    $portmirror.ServiceInsertionRules[0] = [Microsoft.Windows.NetworkController.ServiceInsertionRule]::new()
    $portmirror.ServiceInsertionRules[0].ResourceId = "Rule1"
    $portmirror.ServiceInsertionRules[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionRuleProperties]::new()

    $portmirror.ServiceInsertionRules[0].Properties.Description = "Port Mirror Rule"
    $portmirror.ServiceInsertionRules[0].Properties.Protocol = "All"
    $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeStart = "0"
    $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeEnd = "65535"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeStart = "0"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeEnd = "65535"
    $portmirror.ServiceInsertionRules[0].Properties.SourceSubnets = "*"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationSubnets = "*"

###<a name="step-5-create-a-serviceinsertionelements-object-to-contain-the-network-interface-of-the-appliance-you-are-mirroring-to"></a>Paso 5: Crear un objeto de serviceinsertionelements para incluir la interfaz de red del dispositivo en que se reflejos
Puedes usar los siguientes comandos de ejemplo para crear un objeto de serviceinsertionelements de interfaz de red.

    $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

    $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
    $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
    $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

    $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
    $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
    $portmirror.ServiceInsertionElements[0].Properties.Order = 1

###<a name="step-6-add-the-service-insertion-object-in-network-controller"></a>Paso 6: Agregar el objeto de servicio de inserción en el controlador de red
Al ejecutar este comando, se detendrá todo el tráfico a la interfaz de red de dispositivo especificada en el paso anterior.

Puedes usar los siguientes comandos de ejemplo para agregar el objeto de servicio de inserción en el controlador de red.

    $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"

###<a name="step-7-update-the-network-interface-of-the-source-to-be-mirrored"></a>Paso 7: Actualizar la interfaz de red del origen de reflejarse
Puedes usar los siguientes comandos de ejemplo para actualizar la interfaz de red.

    $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
    $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId

Cuando se han completado estos pasos, el tráfico de la interfaz MyVM_Ethernet1 se refleja de la interfaz Appliance_Ethernet1.
 
