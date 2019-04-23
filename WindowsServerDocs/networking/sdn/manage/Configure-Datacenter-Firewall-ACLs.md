---
title: Configuración de listas de control de acceso (ACL) de Datacenter Firewall
description: Puede aplicar las ACL específicas para las interfaces de red.  Si las ACL también se establecen en la subred virtual a la que está conectada la interfaz de red, tanto las ACL se aplican, pero la ACL de la interfaz de red tienen mayor prioridad por encima de la ACL de subred virtual.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 77a7706e39da265eedd65342a0ccf2174ab050ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853406"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurar listas de Control de acceso (ACL) de firewall de centro de datos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Una vez que ha creado una ACL y asignarlo a una subred virtual, es posible que desee invalidar este comportamiento predeterminado ACL en la subred virtual con una ACL específica para una interfaz de red individuales.  En este caso, se aplican las ACL específicas directamente a las interfaces de red conectadas a redes VLAN, en lugar de la red virtual. Si tiene un conjunto de ACL en la subred virtual conectada a la interfaz de red, tanto las ACL se aplican y da prioridad a la interfaz de red ACL por encima de las ACL de subred virtual.

>[!IMPORTANT]
>Si no ha creado una ACL y asignarlo a una red virtual, consulte [usar Access Control Lists (ACL) para el flujo de tráfico de red de centro de datos de administrar](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) para crear una ACL y asignarla a una subred virtual.  

En este tema, se muestra cómo agregar una ACL a una interfaz de red. También se muestra cómo quitar una ACL de una interfaz de red mediante Windows PowerShell y la API de REST de controladora de red.

- [Ejemplo: Agregar una ACL a una interfaz de red](#example-add-an-acl-to-a-network-interface)
- [Ejemplo: Quitar una ACL de una interfaz de red mediante Windows Powershell y la API de REST de controladora de red](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>Por ejemplo: Agregar una ACL a una interfaz de red
En este ejemplo, se muestra cómo agregar una ACL a una red virtual. 

>[!TIP]
>También es posible agregar una ACL al mismo tiempo que crea la interfaz de red.

1. Obtiene o crea la interfaz de red a la que se agregará la ACL.
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Obtener o crear la ACL que se va a agregar a la interfaz de red.
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. Asignar la ACL a la propiedad AccessControlList de la interfaz de red
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. Agregar la interfaz de red en la controladora de red
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>Por ejemplo: Quitar una ACL de una interfaz de red mediante Windows Powershell y la API de REST de controladora de red
En este ejemplo, se muestra cómo quitar una ACL. Quitar una ACL aplica el conjunto de reglas predeterminado para la interfaz de red. El conjunto predeterminado de reglas permite todo el tráfico saliente pero bloquea todo el tráfico entrante.

>[!NOTE]
>Si desea permitir que todo el tráfico entrante, debe seguir el anterior [ejemplo](#example-add-an-acl-to-a-network-interface) para agregar una ACL que permita el tráfico de todo el tráfico entrante y saliente todo.


1. Obtener la interfaz de red desde el que quitará la ACL.<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Asignar $NULL a la propiedad AccessControlList de la configuración de IP.<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. Agregue el objeto de interfaz de red en la controladora de red.<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
