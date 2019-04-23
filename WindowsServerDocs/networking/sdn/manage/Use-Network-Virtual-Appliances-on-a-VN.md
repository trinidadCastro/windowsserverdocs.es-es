---
title: Usar dispositivos de red virtual en una red virtual
description: En este tema, obtendrá información sobre cómo implementar aplicaciones virtuales de red en redes virtuales de inquilinos. Puede agregar dispositivos virtuales de red a las redes que realizan funciones de creación de reflejo del puerto y el enrutamiento definido por el usuario.
manager: dougkim
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
ms.date: 08/30/2018
ms.openlocfilehash: e715a782651a5b9867f3b45251fd6ea6e4a9e4f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847376"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Usar dispositivos de red virtual en una red virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, obtendrá información sobre cómo implementar aplicaciones virtuales de red en redes virtuales de inquilinos. Puede agregar dispositivos virtuales de red a las redes que realizan funciones de creación de reflejo del puerto y el enrutamiento definido por el usuario.

## <a name="types-of-network-virtual-appliances"></a>Tipos de aplicaciones virtuales de red

Puede usar uno de los dos tipos de aplicaciones virtuales:

1. **Enrutamiento definido por el usuario** -reemplaza distribuidos enrutadores de la red virtual con las funcionalidades de enrutamientos del dispositivo virtual.  Con el enrutamiento definido por el usuario, se usa el dispositivo virtual como un enrutador entre las subredes virtuales en la red virtual.

2. **Creación de reflejo de puerto** : todo tráfico de red que está entrando o abandonar el puerto supervisado se duplicado y se envía a un dispositivo virtual para el análisis. 


## <a name="deploying-a-network-virtual-appliance"></a>Implementar una aplicación virtual de red

Para implementar una aplicación virtual de red, debe en primer lugar, cree una máquina virtual que contiene el dispositivo y, a continuación, conecte la máquina virtual a las subredes de red virtual adecuada. Para obtener más información, consulte [crear una máquina virtual de inquilino y conectarse a una red Virtual del inquilino o VLAN](Create-a-Tenant-VM.md).

Algunos dispositivos requieren varios adaptadores de red virtual. Normalmente, un adaptador de red dedicado a la administración de dispositivo mientras procesan el tráfico de los adaptadores adicionales.  Si su aplicación requiere varios adaptadores de red, debe crear cada interfaz de red en la controladora de red. También debe asignar un identificador de interfaz en cada host para cada uno de los adaptadores adicionales que se encuentran en diferentes subredes virtuales.

Cuando se haya implementado la aplicación virtual de red, puede usar el dispositivo para el enrutamiento definido, la creación de reflejo de portabilidad o ambos. 


## <a name="example-user-defined-routing"></a>Por ejemplo: Enrutamiento definido por el usuario

Para la mayoría de los entornos, solo necesita las rutas del sistema ya definidas por el enrutador distribuido de la red virtual. Sin embargo, deberá crear una tabla de enrutamiento y agregar una o varias rutas en casos concretos, como:

- Tunelización forzada a Internet a través de la red local.
- Uso de aplicaciones virtuales en su entorno.

Para estos escenarios, debe crear una tabla de enrutamiento y agregar las rutas definidas por el usuario a la tabla. Puede tener varias tablas de enrutamientos, y puede asociar la misma tabla de enrutamiento a una o varias subredes. Sólo se puede asociar cada subred a una única tabla de enrutamiento. Todas las máquinas virtuales en una subred utilizan la tabla de enrutamiento asociada a la subred.

Las subredes dependen de las rutas del sistema hasta que se asocia a la subred de una tabla de enrutamiento. Después de que existe una asociación, el enrutamiento se realiza basándose en coincidencia más larga de prefijo (LPM) entre las rutas definidas por el usuario y las rutas del sistema. Si hay más de una ruta con la misma coincidencia LPM, la ruta definida por el usuario se selecciona primero - antes de la ruta del sistema.
 
**Procedimiento:**

1. Cree la ruta de propiedades de tabla, que contiene todas las rutas definidas por el usuario.<p>Las rutas del sistema se siguen aplican según las reglas definidas anteriormente.

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. Agregar una ruta a las propiedades de la tabla de enrutamiento.<p>Cualquier ruta destinado a 12.0.0.0/8 subred se enruta al dispositivo virtual en 192.168.1.10. El dispositivo debe tener un adaptador de red virtual asociado a la red virtual con esa dirección IP asignada a una interfaz de red.

   ```PowerShell
    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route
   ```
   >[!TIP]
   >Si desea agregar varias rutas, repita este paso para cada ruta que va a definir.

3. Agregue la tabla de enrutamiento al controlador de red.

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. Se aplican a la tabla de enrutamiento a la subred virtual.<p>Cuando se aplica a la tabla de rutas a la subred virtual, la primera subred virtual en la red Tenant1_Vnet1 usa la tabla de rutas. Puede asignar la tabla de rutas a todas las subredes de la red virtual como desee.

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

Tan pronto como la tabla de enrutamiento se aplican a la red virtual, el tráfico se reenvía a la aplicación virtual. Debe configurar la tabla de enrutamiento en el dispositivo virtual para reenviar el tráfico, de manera que sea adecuada para su entorno.

## <a name="example-port-mirroring"></a>Por ejemplo: Duplicación de puertos

En este ejemplo, configura el tráfico para MyVM_Ethernet1 reflejado Appliance_Ethernet1.  Se supone que ha implementado dos máquinas virtuales, uno como el dispositivo y la otra como la máquina virtual para supervisar con la creación de reflejo. 

El dispositivo debe tener una segunda interfaz de red para la administración. Después de habilitar la creación de reflejo como un destino en Appliciance_Ethernet1, ya no recibe tráfico destinado a la interfaz IP configurada no existe.


**Procedimiento:**

1. Obtenga la red virtual en el que se encuentran las máquinas virtuales.

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. Obtener las interfaces de red de la controladora de red para el origen y destino de creación de reflejo.

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. Cree un objeto serviceinsertionproperties para que contenga el reglas y el elemento que representa la interfaz de destino de la creación de reflejo del puerto.

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. Cree un objeto serviceinsertionrules para que contenga las reglas que deben cumplirse en orden para el tráfico que se envía al dispositivo.<p>Las reglas definidas por debajo de la coincidencia de todo el tráfico, entrante y saliente, que representa un reflejo tradicional.  Puede ajustar estas reglas si está interesado en un puerto específico o específico de origen y destino de creación de reflejo.

   ```PowerShell
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
   ```

5. Cree un objeto serviceinsertionelements para que contenga la interfaz de red del dispositivo en la reflejada.

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. Agregue el objeto de inserción del servicio de controladora de red.<p>Cuando se emite este comando, todo el tráfico al dispositivo especificado en el anterior deja de paso de interfaz de red.

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. Actualizar la interfaz de red del origen que se puede reflejar.

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

Después de completar estos pasos, la interfaz Appliance_Ethernet1 refleja el tráfico desde la interfaz MyVM_Ethernet1.
 
---