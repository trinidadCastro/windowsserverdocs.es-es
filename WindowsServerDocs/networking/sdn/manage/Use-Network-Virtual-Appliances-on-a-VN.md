---
title: Uso de dispositivos virtuales de red en una red virtual
description: En este tema, aprenderá a implementar aplicaciones virtuales de red en redes virtuales de inquilinos. Puede agregar aplicaciones virtuales de red a redes que realizan funciones de enrutamiento definido por el usuario y de creación de reflejo del puerto.
manager: grcusanz
ms.topic: article
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/30/2018
ms.openlocfilehash: fc263ea778209da699cc7c78fd6111f512760951
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716901"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Uso de dispositivos virtuales de red en una red virtual

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema, aprenderá a implementar aplicaciones virtuales de red en redes virtuales de inquilinos. Puede agregar aplicaciones virtuales de red a redes que realizan funciones de enrutamiento definido por el usuario y de creación de reflejo del puerto.

## <a name="types-of-network-virtual-appliances"></a>Tipos de aplicaciones virtuales de red

Puede usar uno de los dos tipos de aplicaciones virtuales:

1. **Enrutamiento definido por el usuario** : reemplaza los enrutadores distribuidos en la red virtual con las capacidades de enrutamiento del dispositivo virtual.  Con el enrutamiento definido por el usuario, la aplicación virtual se usa como un enrutador entre las subredes virtuales de la red virtual.

2. **Creación de reflejo del puerto** : todo el tráfico de red que entra o sale del puerto supervisado se duplica y se envía a una aplicación virtual para su análisis.


## <a name="deploying-a-network-virtual-appliance"></a>Implementación de una aplicación virtual de red

Para implementar una aplicación virtual de red, primero debe crear una máquina virtual que contenga el dispositivo y, a continuación, conectar la máquina virtual a las subredes de red virtual apropiadas. Para obtener más información, consulte [creación de una máquina virtual de inquilino y conexión a un inquilino Virtual Network o VLAN](Create-a-Tenant-VM.md).

Algunos dispositivos requieren varios adaptadores de red virtual. Normalmente, un adaptador de red dedicado a la administración del dispositivo mientras los adaptadores adicionales procesan el tráfico.  Si el dispositivo requiere varios adaptadores de red, debe crear cada interfaz de red en la controladora de red. También debe asignar un identificador de interfaz en cada host para cada uno de los adaptadores adicionales que se encuentran en subredes virtuales diferentes.

Una vez que haya implementado la aplicación virtual de red, puede usar el dispositivo para el enrutamiento definido, la migración de reflejo o ambos.


## <a name="example-user-defined-routing"></a>Ejemplo: enrutamiento definido por el usuario

Para la mayoría de los entornos, solo necesita las rutas del sistema ya definidas por el enrutador distribuido de la red virtual. Sin embargo, es posible que necesite crear una tabla de enrutamiento y agregar una o varias rutas en casos concretos, como:

- Tunelización forzada a Internet a través de la red local.
- Uso de dispositivos virtuales en su entorno.

En estos casos, debe crear una tabla de enrutamiento y agregar rutas definidas por el usuario a la tabla. Puede tener varias tablas de enrutamiento y puede asociar la misma tabla de enrutamiento a una o varias subredes. Solo puede asociar cada subred a una sola tabla de enrutamiento. Todas las máquinas virtuales de una subred usan la tabla de enrutamiento asociada a la subred.

Las subredes dependen de las rutas del sistema hasta que se asocia una tabla de enrutamiento a la subred. Una vez que existe una asociación, el enrutamiento se realiza según la coincidencia de prefijo más larga (LPM) entre las rutas definidas por el usuario y las rutas del sistema. Si hay más de una ruta con la misma coincidencia de LPM, se selecciona primero la ruta definida por el usuario antes de la ruta del sistema.

**Pasos**

1. Cree las propiedades de la tabla de rutas, que contiene todas las rutas definidas por el usuario.<p>Las rutas del sistema se siguen aplicando según las reglas definidas anteriormente.

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. Agregue una ruta a las propiedades de la tabla de enrutamiento.<p>Cualquier ruta destinada a la subred 12.0.0.0/8 se enruta a la aplicación virtual en 192.168.1.10. El dispositivo debe tener un adaptador de red virtual conectado a la red virtual con esa dirección IP asignada a una interfaz de red.

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
   >Si desea agregar más rutas, repita este paso para cada una de las rutas que desee definir.

3. Agregue la tabla de enrutamiento a la controladora de red.

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. Aplique la tabla de enrutamiento a la subred virtual.<p>Al aplicar la tabla de rutas a la subred virtual, la primera subred virtual de la red Tenant1_Vnet1 usa la tabla de rutas. Puede asignar la tabla de rutas a tantas subredes de la red virtual como desee.

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

En cuanto se aplica la tabla de enrutamiento a la red virtual, el tráfico se reenvía a la aplicación virtual. Debe configurar la tabla de enrutamiento en el dispositivo virtual para reenviar el tráfico de la manera que sea adecuada para su entorno.

## <a name="example-port-mirroring"></a>Ejemplo: creación de reflejo del puerto

En este ejemplo, configurará el tráfico de MyVM_Ethernet1 para reflejar Appliance_Ethernet1.  Damos por hecho que ha implementado dos máquinas virtuales, una como el dispositivo y la otra como la máquina virtual que se va a supervisar con la creación de reflejo.

El dispositivo debe tener una segunda interfaz de red para la administración. Después de habilitar la creación de reflejo como destino en Appliciance_Ethernet1, ya no recibe el tráfico destinado a la interfaz IP configurada ahí.


**Pasos**

1. Obtenga la red virtual en la que se encuentran las máquinas virtuales.

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. Obtiene las interfaces de red de la controladora de red para el origen y el destino de la creación de reflejo.

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. Cree un objeto serviceinsertionproperties que contenga las reglas de creación de reflejo del puerto y el elemento que representa la interfaz de destino.

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. Cree un objeto serviceinsertionrules para que contenga las reglas que deben coincidir para que el tráfico se envíe al dispositivo.<p>Las reglas que se definen a continuación coinciden con todo el tráfico, tanto de entrada como de salida, que representa un reflejo tradicional.  Puede ajustar estas reglas si está interesado en crear un reflejo de un puerto específico o en orígenes o destinos específicos.

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

5. Cree un objeto serviceinsertionelements que contenga la interfaz de red del dispositivo reflejado.

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. Agregue el objeto de inserción de servicio en la controladora de red.<p>Cuando se emite este comando, se detiene todo el tráfico a la interfaz de red del dispositivo especificado en el paso anterior.

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. Actualice la interfaz de red del origen que se va a reflejar.

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

Después de completar estos pasos, la interfaz de Appliance_Ethernet1 refleja el tráfico de la interfaz de MyVM_Ethernet1.

---