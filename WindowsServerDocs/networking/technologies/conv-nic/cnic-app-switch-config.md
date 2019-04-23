---
title: Configuración del conmutador físico de NIC convergente
description: En este tema, se proporcionan directrices para configurar los conmutadores físicos.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: e31d7b83fee84d9055d938f77b49389205786244
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829406"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuración del conmutador físico de NIC convergente

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se proporcionan directrices para configurar los conmutadores físicos. 


Estos son sólo los comandos y sus usos; debe determinar los puertos a la que está conectada la NIC en su entorno. 

>[!IMPORTANT]
>Asegúrese de que la VLAN y la directiva de no colocar está establecido para la prioridad sobre el que se configura SMB.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Conmutador de arista \(controladores de dominio\-7050s\-EOS 64,\-4.13.7M\)

1.  en \(ir al modo de administrador, normalmente solicitará una contraseña\)
2.  configuración \(entrar en modo de configuración\)
3.  Mostrar ejecución \(muestra la configuración de ejecución actual\)
4.  Descubra los puertos de conmutador al que está conectada las NIC. En estos ejemplos, son 14/1,15/1,16/1,17/1.
5.  int eth 14/1,15/1,16/1,17/1 \(entrar en modo de configuración para estos puertos\)
6.  modo dcbx ieee
7.  en el modo de control de flujo de prioridad
8.  switchport tronco de vlan nativo 225
9.  switchport tronco permite vlan 100-225
10. tronco de modo switchport
11. prioridad de control de flujo de prioridad 3 no colocar
12. calidad de servicio de confianza cos
13. Mostrar ejecución \(Compruebe que la configuración está configurado correctamente en los puertos\)
14. wR \(para que la configuración se mantiene en el reinicio de conmutador\)

### <a name="tips"></a>Sugerencias:
1.  No hay command # niega un comando
2.  Cómo agregar una nuevo VLAN: int vlan 100 \(si la red de almacenamiento está en la VLAN 100\)
3.  Cómo comprobar las VLAN existentes: mostrar vlan
4.  Para obtener más información sobre cómo configurar el conmutador de Arista, busque en línea: Manual de arista EOS
5.  Use este comando para comprobar la configuración de PFC: mostrar los detalles de los contadores de control de flujo de prioridad

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Conmutador de Dell \(S4810, 9,9 FTOS \(0.0\)\)

    
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

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Conmutador Cisco \(Nexus 3132, versión 6.0\(2\)U6\(1\)\)

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
    
--- 

## <a name="related-topics"></a>Temas relacionados

- [Configuración de NIC convergentes con un único adaptador de red](cnic-single.md)
- [Configuración de NIC convergente NIC asociadas](cnic-datacenter.md)
- [Solución de problemas convergente configuraciones de NIC](cnic-app-troubleshoot.md)

--- 