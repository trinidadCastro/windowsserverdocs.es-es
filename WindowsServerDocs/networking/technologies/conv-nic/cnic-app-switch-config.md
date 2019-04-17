---
title: Configuración del conmutador físico para convergente NIC
description: Este tema es parte de la convergido NIC configuración guía para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 98a2e249aea38bd4d07dc1bcbc9b1ca98b98b6d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuración del conmutador físico para convergente NIC

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar las siguientes secciones como instrucciones para configurar los conmutadores físicos.

Estos son solo los comandos y sus usos; debes determinar los puertos a la que están conectadas el NIC en su entorno. 

>[!IMPORTANT]
>Asegúrate de que la directiva de no colocar y VLAN está definida para la prioridad sobre la que está configurado SMB.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Conmutador de arista \ (dcs\-7050s\-64, EOS\-4.13.7M\)

1.  en \ (Ir al modo de administrador, normalmente solicita un password\)
2.  configuración \ (para entrar en modo de configuración a errores\)
3.  Mostrar ejecución \ (muestra configuration\ actual de ejecución)
4.  descubre los puertos de cambiar a la que la NIC están conectadas a. En estos ejemplo, son 14, 1,15, 1,16, 1,17/1.
5.  int eth 14, 1,15, 1,16, 1,17/1 \ (escribe en el modo de configuración para estos ports\)
6.  modo dcbx ieee
7.  en el modo de control de flujo de prioridad
8.  switchport tronco nativo vlan 225
9.  switchport tronco permitido vlan 100-225
10. switchport troncal de modo
11. prioridad 3 de control de flujo de prioridad no colocar
12. QoS confianza cos
13. Mostrar ejecución \ (comprobar que la configuración está configurado correctamente en el ports\)
14. wR \ (para que la configuración se mantiene con todas conmutador reboot\)

### <a name="tips"></a>Sugerencias:
1.  No command # niega un comando
2.  Cómo agregar una nueva VLAN: int vlan 100 \ (si el almacenamiento de red es en VLAN 100\)
3.  Cómo comprobar VLAN existentes: mostrar vlan
4.  Para obtener más información sobre cómo configurar Arista conmutador, buscar en línea: Manual de fin de soporte de Arista
5.  Usa este comando para comprobar la configuración de PFC: mostrar los detalles de los contadores de control de flujo de prioridad

## <a name="dell-switch-s4810-ftos-99-00"></a>Conmutador de Dell \ (S4810, FTOS 9,9 \(0.0\)\)

    
    !
    dcb enable
    ! put pfc control on qos class 3
    configure
    dcb-map dcb-smb
    priority group 0 bandwidth 90 pfc on
    priority group 1 bandwidth 10 pfc off
    priority-pgid 1 1 1 0 1 1 1 1
    exit
    ! apply map to ports 0-31
    configure
    interface range ten 0/0-31
    dcb-map dcb-smb
    exit
    

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Conmutador de Cisco \ (nexo 3132, versión 6.0\(2\)U6\(1\)\)

### <a name="global"></a>Global
    
    class-map type qos match-all RDMA
    match cos 3
    class-map type queuing RDMA
    match qos-group 3
    policy-map type qos QOS_MARKING
    class RDMA
    set qos-group 3
    class class-default
    policy-map type queuing QOS_QUEUEING
    class type queuing RDMA
    bandwidth percent 50
    class type queuing class-default
    bandwidth percent 50
    class-map type network-qos RDMA
    match qos-group 3
    policy-map type network-qos QOS_NETWORK
    class type network-qos RDMA
    mtu 2240
    pause no-drop
    class type network-qos class-default
    mtu 9216
    system qos
    service-policy type qos input QOS_MARKING
    service-policy type queuing output QOS_QUEUEING
    service-policy type network-qos QOS_NETWORK
    

### <a name="port-specific"></a>Puerto específico

    
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
    

## <a name="all-topics-in-this-guide"></a>Todos los temas de esta guía

Esta guía contiene los siguientes temas.

- [Configuración de NIC convergente con un adaptador de red](cnic-single.md)
- [Configuración de NIC convergente NIC de equipo](cnic-datacenter.md)
- [Configuración del conmutador físico para convergente NIC](cnic-app-switch-config.md)
- [Solución de problemas convergido configuraciones NIC](cnic-app-troubleshoot.md)
