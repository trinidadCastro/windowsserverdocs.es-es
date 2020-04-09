---
title: Configuración de listas de control de acceso (ACL) de Datacenter Firewall
description: Puede aplicar ACL específicas a las interfaces de red.  Si también se establecen ACL en la subred virtual a la que está conectada la interfaz de red, se aplican ambas ACL, pero las ACL de la interfaz de red se priorizan por encima de las ACL de la subred virtual.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: f6b1078f88b2d377c3c49934e2b1bd219641d82e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854568"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurar listas de Access Control de Firewall de centro de seguridad (ACL)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Una vez que haya creado una ACL y la haya asignado a una subred virtual, puede que desee invalidar esa ACL predeterminada en la subred virtual con una ACL específica para una interfaz de red individual.  En este caso, se aplican ACL específicas directamente a las interfaces de red conectadas a redes VLAN, en lugar de a la red virtual. Si tiene ACL establecidas en la subred virtual conectada a la interfaz de red, se aplican ambas ACL y se priorizan las ACL de la interfaz de red por encima de las ACL de la subred virtual.

>[!IMPORTANT]
>Si no ha creado una ACL y la ha asignado a una red virtual, consulte [uso de listas de Access Control (ACL) para administrar el flujo de tráfico de red del centro](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) de información para crear una ACL y asignarla a una subred virtual.  

En este tema, se muestra cómo agregar una ACL a una interfaz de red. También le mostramos cómo quitar una ACL de una interfaz de red mediante Windows PowerShell y la API de REST de la controladora de red.

- [Ejemplo: agregar una ACL a una interfaz de red](#example-add-an-acl-to-a-network-interface)
- [Ejemplo: quitar una ACL de una interfaz de red mediante Windows PowerShell y la API de REST de la controladora de red](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>Ejemplo: agregar una ACL a una interfaz de red
En este ejemplo, se muestra cómo agregar una ACL a una red virtual. 

>[!TIP]
>También es posible agregar una ACL al mismo tiempo que se crea la interfaz de red.

1. Obtenga o cree la interfaz de red a la que agregará la ACL.
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Obtiene o crea la ACL que agregará a la interfaz de red.
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. Asignación de la ACL a la propiedad AccessControlList de la interfaz de red
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. Agregar la interfaz de red en la controladora de red
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>Ejemplo: quitar una ACL de una interfaz de red mediante Windows PowerShell y la API de REST de la controladora de red
En este ejemplo, se muestra cómo quitar una ACL. Al quitar una ACL se aplica el conjunto predeterminado de reglas a la interfaz de red. El conjunto predeterminado de reglas permite todo el tráfico saliente, pero bloquea todo el tráfico entrante.

>[!NOTE]
>Si desea permitir todo el tráfico entrante, debe seguir el [ejemplo](#example-add-an-acl-to-a-network-interface) anterior para agregar una ACL que permita todo el tráfico entrante y saliente.


1. Obtenga la interfaz de red desde la que se quitará la ACL.<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Asigne $NULL a la propiedad AccessControlList de la ipConfiguration.<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. Agregue el objeto de interfaz de red en la controladora de red.<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
