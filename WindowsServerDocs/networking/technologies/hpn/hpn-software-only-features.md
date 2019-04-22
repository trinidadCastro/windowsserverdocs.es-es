---
title: Funciones y tecnologías de solo software
description: Estas características se implementan como parte del sistema operativo y son independientes de la NIC subyacente. A veces, estas características requieren algunos ajustes de la NIC para un funcionamiento óptimo. Estos ejemplos de características de Hyper-v como calidad de servicio (vmQoS) de máquina Virtual, listas de Control de acceso (ACL) y las características de Hyper-V como formación de equipos NIC.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 504bc92971e778b468812dc4064fa6f0afff87ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823786"
---
# <a name="software-only-so-features-and-technologies"></a>Funciones y tecnologías de solo software
Características del software sólo se implementan como parte del sistema operativo y son independientes de la NIC subyacente. A veces, estas características requieren algunos ajustes de la NIC para un funcionamiento óptimo. Estos ejemplos de características de Hyper-v como calidad de servicio (vmQoS) de máquina Virtual, listas de Control de acceso (ACL) y las características de Hyper-V como formación de equipos NIC.

## <a name="access-control-lists-acls"></a>Listas de Control de acceso (ACL)

Una característica de Hyper-V y SDNv1 para administrar la seguridad de una máquina virtual. Esta característica se aplica a la pila de Hyper-V no virtualizados y la pila HVNv1. Puede administrar Hyper-v. cambiar la ACL a través de [Add-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps) y [Remove-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) cmdlets de PowerShell.

## <a name="extended-acls"></a>ACL extendidas

Conmutador Virtual de Hyper-V ACL extendidas le permiten configurar el Hyper-V Virtual Switch extendido ACL de puerto para proporcionar protección de firewall y aplicar directivas de seguridad para las máquinas virtuales de inquilino en centros de datos. Dado que las ACL de puerto se configuran en el conmutador Virtual de Hyper-V en lugar de dentro de las máquinas virtuales, el administrador puede administrar las directivas de seguridad para todos los inquilinos en un entorno de varios inquilinos.

Puede administrar Hyper-V ACL extendidas del conmutador a través de la [Add-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps) y [Remove-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) cmdlets de PowerShell.

>[!TIP] 
>Esta característica se aplica a la pila HNVv1. Las ACL en la pila de SDN, consulte definidas por Software de red SDN) ACL a continuación.

Para obtener más información acerca de Extended puerto Access Control Lists en esta biblioteca, consulte [crear directivas de seguridad con listas de Control de acceso de puerto de extendido](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists).

## <a name="nic-teaming"></a>Formación de equipos NIC

Formación de equipos NIC, también denominada unión de NIC, es la agregación de varios puertos NIC en una entidad en que el host se percibe como un único puerto NIC. Formación de equipos NIC protege contra los errores de un solo puerto NIC (o el cable conectado a ella). También agrega el tráfico de red para un rendimiento más rápido. Para obtener más información, consulte [formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

Tiene dos formas de realizar la formación de equipos con Windows Server 2016:

1.  Solución de formación de equipos de Windows Server 2012

2.  Windows Server 2016 Switch Embedded Teaming (SET)


## <a name="rsc-in-the-vswitch"></a>RSC en el vSwitch

Recibir segmento Coalescing (RSC) en el vSwitch es una característica que toma los paquetes que forman parte de la misma secuencia y llegan entre las interrupciones de red y fusiona en un único paquete antes de enviarlos al sistema operativo. El conmutador virtual en Windows Server 2019 tiene esta característica. Para obtener más detalles sobre esta característica, consulte [fusión de segmentos de recepción en el vSwitch](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).

## <a name="software-defined-networking-sdn-acls"></a>Las ACL de red (SDN) definidas por software

La extensión de SDN en Windows Server 2016 mejora formas para adecuarse a las ACL. En la pila de SDN de Windows Server 2016 v2, se usan las ACL de SDN en lugar de ACL y ACL extendida. Puede usar controladora de red para administrar las ACL de SDN. 

## <a name="sdn-quality-of-service-qos"></a>Calidad de servicio (QoS) de SDN

La extensión SDN en Windows Server 2016 mejora formas de proporcionar control de ancho de banda (límites de entrada, límites de salida y las reservas de direcciones de salida) en una base de 5-tupla. Normalmente, estas directivas se aplicarán en el nivel de vNIC o vmNIC, pero puede hacerlos mucho más específicas. En la pila de SDN de Windows Server 2016 v2, QoS de SDN se usa en lugar de vmQoS. Puede usar controladora de red para administrar la QoS de SDN.

## <a name="switch-embedded-teaming-set"></a>Switch Embedded Teaming (SET)

CONJUNTO es una solución alternativa de formación de equipos NIC que puede usar en entornos que incluyen Hyper-V y la pila de redes definidas por Software (SDN) en Windows Server 2016. CONJUNTO integra alguna funcionalidad de formación de equipos NIC en el conmutador Virtual de Hyper-V. Para obtener información sobre Switch Embedded Teaming en esta biblioteca, consulte [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="virtual-receive-side-scaling-vrss"></a>Ajuste de escala en lado de recepción virtual (vRSS)

Software vRSS se utiliza para distribuir el tráfico entrante destinado a una máquina virtual entre varios procesadores lógicos (LPs) de la máquina virtual. VRSS software proporciona a la máquina virtual sería capaz de controlar la capacidad para manejar más tráfico de red que un único LP. Para obtener más información, consulte [Virtual escala de recepción (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top).

## <a name="virtual-machine-quality-of-service-vmqos"></a>Calidad de servicio (vmQoS) de máquina virtual

Calidad de servicio de máquina virtual es una característica de Hyper-V que permite el modificador establecer límites en el tráfico generado por cada máquina virtual. También permite a una máquina virtual reservar una cantidad de ancho de banda en la conexión de red externo para que una máquina virtual no puede privar a otra máquina virtual para el ancho de banda. En la pila de SDN de Windows Server 2016 v2, QoS de SDN reemplaza vmQoS.

vmQoS puede establecer límites de salida y las reservas de direcciones de salida. Antes de crear el conmutador de Hyper-V debe determinar el modo de reserva de salida (peso relativo o absoluto ancho de banda).

-  Determinar el modo de reserva de salida con el parámetro – MinimumBandwidthMode del cmdlet de PowerShell New-VMSwitch.

-  Establezca el valor del límite de salida con el parámetro – MaximumBandwidth en el cmdlet de PowerShell Set-VMNetworkAdapter.

-  Establezca el valor de la reserva de salida con cualquiera de los siguientes parámetros del cmdlet de VMNetworkAdapter PowerShell establecer:

   -  Si el parámetro – MinimumBandwidthMode sobre el cmdlet New-VMSwitch es absoluto, a continuación, establezca el parámetro – MinimumBandwidthAbsolute sobre el cmdlet establezca VMNetworkAdapter.

   -  Si el parámetro – MinimumBandwidthMode sobre el cmdlet New-VMSwitch es el peso, a continuación, establezca el parámetro – parámetros MinimumBandwidthWeight sobre el cmdlet establezca VMNetworkAdapter.

Debido a las limitaciones en el algoritmo utilizado para esta característica, se recomienda que el peso más alto o ancho de banda absoluto no sea más de 20 veces el peso más bajo o ancho de banda absoluto. Si necesita más control, considere la posibilidad de usar la pila de SDN y la característica de QoS de SDN.


---