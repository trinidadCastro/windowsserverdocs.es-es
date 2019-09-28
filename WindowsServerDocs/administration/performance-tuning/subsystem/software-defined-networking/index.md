---
title: Optimización del rendimiento de redes definidas por software
description: Directrices de optimización del rendimiento de redes definidas por software (SDN)
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8227c94e6785f4acf9135aac12406b6ac98a0910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383523"
---
# <a name="performance-tuning-software-defined-networks"></a>Optimización del rendimiento de redes definidas por software

Las redes definidas por software (SDN) en Windows Server 2016 están conformadas por una combinación de una controladora de red, hosts de Hyper-V, puertas de enlace de equilibrador de carga de software y puertas de enlace HNV.  Para optimizar cada uno de estos componentes, consulte las secciones siguientes:

## <a name="network-controller"></a>Controladora de red

La controladora de red es un rol de Windows Server que se debe habilitar en las máquinas virtuales que se ejecutan en los hosts que están configurados para usar SDN y controlados por la controladora de red.

Tres máquinas virtuales habilitadas por controladora de red son suficientes para obtener una alta disponibilidad y el máximo rendimiento.  El tamaño de cada máquina virtual se debe ajustar según las directrices que se proporcionan en la sección de requisitos del rol de máquina virtual de la infraestructura de SDN del tema [Planeación de una infraestructura de red definida por software](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).

### <a name="sdn-quality-of-service-qos"></a>Calidad de servicio (QoS) de SDN

Para garantizar que el tráfico de máquina virtual tiene la prioridad correcta, se recomienda configurar la calidad de servicio de SDN en las máquinas virtuales de carga de trabajo.  Para más información sobre cómo configurar la calidad de servicio de SDN, consulte el tema [Configurar la calidad de servicio para un adaptador de red de máquina virtual de inquilino](../../../../networking/sdn/manage/Configure-QoS-for-Tenant-VM-Network-Adapter.md).

## <a name="hyper-v-host-networking"></a>Redes de host de Hyper-V

Las indicaciones que se proporciona en la sección de rendimiento de E/S de una red de Hyper-V de la guía [Ajuste del rendimiento para servidores Hyper-V](../../role/remote-desktop/session-hosts.md) se aplican cuando se usa SDN, pero esta sección abarca directrices adicionales que se deben seguir para garantizar el mejor rendimiento al usar SDN.

### <a name="physical-network-adapter-nic-teaming"></a>Formación de equipos NIC

Para obtener el mejor rendimiento y las funcionalidades de conmutación por error, se recomienda configurarlos adaptadores de red físicos para que formen equipos.  Cuando use SDN, debe crear el equipo con Switch Embedded Teaming (SET).  

El número óptimo de miembros del equipo es dos, porque el tráfico virtualizado se distribuirá entre ambos miembros del equipo tanto para las direcciones de entrada como de salida.  Puede haber más de dos miembros del equipo, pero el tráfico de entrada se distribuirá a lo sumo a dos de los adaptadores.  El tráfico de salida siempre se distribuirá entre todos los adaptadores si el valor predeterminado del equilibrio dinámico de carga sigue configurado en el conmutador virtual.


### <a name="encapsulation-offloads"></a>Descargas de encapsulación

SDN se basa en la encapsulación de paquetes para virtualizar la red.  Para obtener un rendimiento óptimo, es importante que el adaptador de red admita la sobrecarga de hardware correspondiente al formato de encapsulación que se usa.  No hay ninguna ventaja de rendimiento importante de un formato de encapsulación sobre otro.  El formato de encapsulación predeterminado cuando se usa la controladora de red es VXLAN.

Puede terminar qué formato de encapsulación se usa a través de la controladora de red con el cmdlet de PowerShell siguiente:

``` syntax
    (Get-NetworkControllerVirtualNetworkConfiguration -connectionuri $uri).properties.networkvirtualizationprotocol
```

Para obtener el mejor rendimiento, si se devuelve VXLAN, debe asegurarse de que los adaptadores de red física admitan la descarga de tareas VXLAN.  Si se devuelve NVGRE, los adaptadores de red física deben admitir la descarga de tareas NVGRE.

### <a name="mtu"></a>Unidad de transmisión máxima

La encapsulación genera que se agreguen bytes adicionales a cada paquete.  Con el fin de evitar la fragmentación de estos paquetes, la red física se debe configurar para el uso de tramas gigantes.  Un valor de MTU de 9234 es el tamaño recomendado para VXLAN o NVGRE y se debe configurar en el conmutador físico para las interfaces físicas de los puertos de host (L2) y las interfaces de enrutador (L3) de las VLAN en la que se enviarán los paquetes encapsulados.  Esto incluye las redes de tránsito, proveedor de HNV y administración.

MTU en el host de Hyper-V se configura a través del adaptador de red y el agente de host de la controladora de red que se ejecuta en el host de Hyper-V se ajustará para la sobrecarga de encapsulación de manera automática si es compatible con el controlador de adaptador de red.  

Una vez que el tráfico sale de la red virtual a través de una puerta de enlace, la encapsulación se quita y se usa la MTU original tal como se envió desde la máquina virtual.

### <a name="single-root-io-virtualization-sr-iov"></a>Virtualización de E/S de raíz única (SR-IOV)

SDN se implementa en el host de Hyper-V mediante una extensión de conmutador de reenvío en el conmutador virtual.  Para que esta extensión de conmutador procese los paquetes, no se debe usar SR-IOV en interfaces de red virtual que estén configuradas para usarlas con la controladora de red, ya que hace que el tráfico de máquina virtual omita el conmutador virtual.

SR-IOV de todos modos se puede habilitar en el conmutador virtual si así se desea y lo pueden usar los adaptadores de red de máquina virtual no controlados por la controladora de red.  Estas máquinas virtuales de SR-IOV pueden coexistir en el mismo conmutador virtual que las máquinas virtuales controladas por la controladora virtual que no usan SR-IOV.

Si usa adaptadores de red de 40 Gbit, se recomienda habilitar SR-IOV en el conmutador virtual de las puertas de enlace de equilibrio de carga de software (SLB) para alcanzar el rendimiento máximo.  Este tema se trata con más detalles en la sección [Puertas de enlace de equilibrador de carga de software](slb-gateway-performance.md).

## <a name="hnv-gateways"></a>Puertas de enlace HNV

Puede encontrar información sobre cómo optimizar las puertas de enlace HNV para usarlas con SDN en la sección [Puertas de enlace HNV](hnv-gateway-performance.md).

## <a name="software-load-balancer-slb"></a>Equilibrador de carga de software (SLB)

Las puertas de enlace de SLB solo se pueden usar con la controladora de red y SDN.  Puede encontrar más información sobre cómo ajustar SDN para usarlo con las puertas de enlace de SLB en la sección [Puertas de enlace de equilibrador de carga de software](slb-gateway-performance.md).
