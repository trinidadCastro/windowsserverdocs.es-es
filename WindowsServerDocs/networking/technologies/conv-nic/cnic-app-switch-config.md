---
title: Configuración del conmutador físico para la NIC convergente
description: En este tema se proporcionan instrucciones para configurar los conmutadores físicos.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: d10e8ca6e4689b89a8b9532f77613f17280282b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355474"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuración del conmutador físico para la NIC convergente

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporcionan instrucciones para configurar los conmutadores físicos. 


Estos son solo comandos y sus usos; debe determinar los puertos en los que las NIC están conectadas en su entorno. 

>[!IMPORTANT]
>Asegúrese de que la Directiva de VLAN y no de eliminación está establecida para la prioridad en la que se configura SMB.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista modificador \(dcs @ no__t-17050s @ no__t-264, EOS @ no__t-34.13.7 M @ no__t-4

1.  en \(go al modo de administrador, normalmente solicita una contraseña @ no__t-1
2.  config \(to entrar en el modo de configuración @ no__t-1
3.  Mostrar @no__t de ejecución-0shows configuración actual en ejecución @ no__t-1
4.  Obtenga información sobre los puertos de conmutador a los que están conectados las NIC. En estos ejemplos, son 14/1, 15/1, 16/1, 17/1.
5.  int ETH 14/1, 15/1, 16/1, 17/1 \(enter en modo de configuración para estos puertos @ no__t-1
6.  IEEE de modo dcbx
7.  modo de control de flujo de prioridad activado
8.  enlace de switchport Native VLAN 225
9.  tronco troncal permitido VLAN 100-225
10. tronco del modo de switchport
11. Priority-Flow-Control Priority 3 no-Drop
12. confianza de QoS cos
13. Mostrar ejecutar \(verify que la configuración está configurada correctamente en los puertos @ no__t-1
14. WR \(to hacer que la configuración se mantenga en el reinicio del conmutador @ no__t-1

### <a name="tips"></a>Útiles
1.  No #command # niega un comando
2.  Cómo agregar una nueva VLAN: int VLAN 100 \(If Storage Network está en VLAN 100 @ no__t-1
3.  Comprobación de VLAN existentes: Mostrar VLAN
4.  Para obtener más información acerca de cómo configurar el modificador arista, busque: Arista manual de EOS
5.  Use este comando para comprobar la configuración de PFC: mostrar detalles de contadores de control de flujo de prioridad

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Conmutador Dell \(S4810, FTOS 9,9 \(0.0 @ no__t-2 @ no__t-3

    
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
    
--- 

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Cisco switch \(Nexus 3132, versión 6.0 @ no__t-12 @ no__t-2U6 @ no__t-31 @ no__t-4 @ no__t-5

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
    

### <a name="port-specific"></a>Específico del puerto

    
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
    
--- 

## <a name="related-topics"></a>Temas relacionados

- [Configuración de NIC convergente con un solo adaptador de red](cnic-single.md)
- [Configuración de NIC en equipo NIC convergente](cnic-datacenter.md)
- [Solución de problemas de configuraciones de NIC convergentes](cnic-app-troubleshoot.md)

--- 