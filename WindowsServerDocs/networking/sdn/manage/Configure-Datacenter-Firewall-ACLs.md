---
title: Configurar listas de Control de acceso (ACL) de Datacenter Firewall
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: d5f7c4baad24a720e073857cb6c835167e5b419b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurar listas de Control de acceso (ACL) de Datacenter Firewall

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes aplicar ACL específicas a interfaces de red.  Si ACL también se establecen en la subred virtual a la que está conectada a la interfaz de red, tanto se aplican ACL, pero se da prioridad a la ACL de la interfaz de red por encima de la subred virtual ACL.

Este tema contiene las siguientes secciones.

- [Ejemplo: Agregar una ACL a una interfaz de red](#bkmk_addacl)
- [Ejemplo: Quitar una ACL de una interfaz de red mediante Windows Powershell y la API de REST de controlador de red](#bkmk_removeacl)

##<a name="bkmk_addacl"></a>Ejemplo: Agregar una ACL a una interfaz de red

En el tema [usa acceso listas de Control (ACL) para hacer fluir el tráfico de red de Datacenter administrar](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) has aprendido a crear una ACL y asignarlo a una subred virtual.  Sin embargo, en algunos casos, es posible que quieras anular ese valor predeterminado ACL de la subred virtaul con una ACL específicas para una interfaz de red individuales.  También deberás aplicar ACL directamente a interfaces de red que se adjuntan VLANs en lugar de redes virtuales.

Este ejemplo muestra cómo agregar una ACL a una red virtual. 

>[!NOTE]
>También es posible agregar una ACL al mismo tiempo que creas la interfaz de red.

###<a name="step-1-get-or-create-the-network-interface-to-which-you-will-add-the-acl"></a>Paso 1: Obtener o crear la interfaz de red al que se agregará la ACL

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-get-or-create-the-acl-you-will-add-to-the-network-interface"></a>Paso 2: Obtener o crear la ACL que se agregará a la interfaz de red
Puedes usar el siguiente comando de ejemplo para obtener o crear la ACL. 

    $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"

###<a name="step-3-assign-the-acl-to-the-accesscontrollist-property-of-the-network-interface"></a>Paso 3: Asignar la ACL a la propiedad AccessControlList de la interfaz de red
Puedes usar el siguiente comando de ejemplo para asignar la ACL a la propiedad AccessControlList.

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl

###<a name="step-4-add-the-network-interface-in-network-controller"></a>Paso 4: Agregar la interfaz de red en el controlador de red
Puedes usar el siguiente comando de ejemplo para agregar la interfaz de red en el controlador de red.

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid


##<a name="bkmk_removeacl"></a>Ejemplo: Quitar una ACL de una interfaz de red mediante Windows Powershell y la API de REST de controlador de red
Para usar este ejemplo, si desea quitar una ACL. Cuando se quita una ACL, el conjunto predeterminado de reglas se aplican a la interfaz de red.

El conjunto predeterminado de reglas permite todo el tráfico saliente, pero bloquea todo el tráfico entrante.

>[!NOTE]
>Si desea permitir todo el tráfico entrante, debes seguir el ejemplo anterior para agregar una ACL que permite todo el tráfico entrante y saliente de todo.

###<a name="step-1-get-the-network-interface-from-which-you-will-remove-the-acl"></a>Paso 1: Obtener la interfaz de red desde el que se quite la ACL
Puedes usar el siguiente comando de ejemplo para recuperar la interfaz de red.

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-assign-null-to-the-accesscontrollist-property-of-the-ipconfiguration"></a>Paso 2: Asignar $NULL a la propiedad AccessControlList la IP
Puedes usar el siguiente comando de ejemplo para asignar $NULL a la propiedad AccessControlList.

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $null

###<a name="step-3-add-the-network-interface-object-in-network-controller"></a>Paso 3: Agregar el objeto de la interfaz de red en el controlador de red
Puedes usar el siguiente comando de ejemplo para agregar el objeto de la interfaz de red en el controlador de red,

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid

