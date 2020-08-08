---
title: Novedades de virtualización de red de Hyper-V en Windows Server 2016
description: En este tema se proporciona información acerca de las nuevas características de virtualización de red de Hyper-V en Windows Server 2016
manager: grcusanz
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: anpaul
author: AnirbanPaul
ms.date: 03/19/2018
ms.openlocfilehash: 9daab9358cbfd337b755605e7233b1bcea555f22
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994267"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Novedades de virtualización de red de Hyper-V en Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describen las funciones de virtualización de red de Hyper-V (HNV) nuevas o modificadas en Windows Server 2016.

## <a name="updates-in-hnv"></a><a name="BKMK_IPAM2012R2"></a>Actualizaciones en HNV
HNV ofrece compatibilidad mejorada en las áreas siguientes:

|Característica/función|Nueva o mejorada|Descripción|
|--------------------------|-------------------|---------------|
|[Conmutador de Hyper-V programable](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nuevo|La Directiva HNV se programa a través de la controladora de red de Microsoft.|
|[Compatibilidad con la encapsulación de VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nuevo|HNV ahora admite la encapsulación VXLAN.|
|[Interoperabilidad de Load Balancer de software (SLB)](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nuevo|HNV está totalmente integrado con el software de Microsoft Load Balancer.|
|[Encabezados IEEE Ethernet compatibles](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Se ha mejorado|Compatible con los estándares IEEE Ethernet|

### <a name="programmable-hyper-v-switch"></a><a name="SDN"></a>Conmutador de Hyper-V programable
HNV es un bloque de creación fundamental de la solución de redes definidas por software (SDN) actualizada de Microsoft y está totalmente integrado en la pila de SDN.

El nuevo controlador de red de Microsoft envía directivas de HNV a un agente de host que se ejecuta en cada host mediante Open vSwitch Database Management Protocol (OVSDB) como la interfaz SouthBound (SBI). El agente de host almacena esta Directiva mediante una personalización del [esquema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) y ejecuta reglas de flujo complejo en un motor de flujo de rendimiento en el conmutador de Hyper-V.

El motor de flujo dentro del conmutador de Hyper-V es el mismo motor que se usa en Microsoft Azure &trade; , que se ha demostrado a hiperescala en la nube pública Microsoft Azure. Además, toda la pila de SDN a través de la controladora de red y el proveedor de recursos de red (los detalles próximamente) son coherentes con Microsoft Azure, lo que aporta la eficacia de la nube Microsoft Azure a nuestros clientes empresariales y proveedores de servicios de hosting.

> [!NOTE]
> Para obtener más información sobre OVSDB, consulte [RFC 7047](https://www.rfc-editor.org/info/rfc7047).

El conmutador de Hyper-V admite reglas de flujo sin estado y con estado basadas en la "acción de coincidencia" simple en el motor de flujo de Microsoft.

![Conmutador de Hyper-V de Windows Server 2016](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)

### <a name="vxlan-encapsulation-support"></a><a name="VXLAN"></a>Compatibilidad con la encapsulación de VXLAN
El protocolo de red de área local extensible (VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)) se ha adoptado ampliamente en el mercado, con soporte técnico de proveedores como Cisco, Brocade, Dell, HP y otros. HNV también admite ahora este esquema de encapsulación mediante el modo de distribución de MAC a través de la controladora de red de Microsoft para las asignaciones de las direcciones IP de red de superposición de inquilinos (dirección de cliente o CA) a las direcciones IP de red de proporcionaban física (dirección de proveedor o PA). Se admiten descargas de tareas NVGRE y VXLAN para mejorar el rendimiento a través de controladores de terceros.

### <a name="software-load-balancer-slb-interoperability"></a><a name="SLB"></a>Interoperabilidad de Load Balancer de software (SLB)
Windows Server 2016 incluye un equilibrador de carga de software (SLB) con compatibilidad total para el tráfico de red virtual y la interacción sin problemas con HNV. El SLB se implementa a través del motor de flujo de rendimiento en el conmutador v-switch Data plano y controlado por la controladora de red para las asignaciones IP virtuales (VIP)/IP dinámicas (DIP).

### <a name="compliant-ieee-ethernet-headers"></a><a name="L2"></a>Encabezados IEEE Ethernet compatibles
HNV implementa encabezados Ethernet L2 correctos para garantizar la interoperabilidad con dispositivos físicos y virtuales de terceros que dependen de los protocolos estándar del sector. Microsoft garantiza que todos los paquetes transmitidos tienen valores compatibles en todos los campos para garantizar esta interoperabilidad. Además, se necesitará compatibilidad con tramas gigantes (MTU > 1780) en la red L2 física para tener en cuenta la sobrecarga de paquetes introducida por los protocolos de encapsulación (NVGRE, VXLAN) al tiempo que se garantiza que los invitados Virtual Machines conectados a un HNV Virtual Network mantener una MTU de 1514.

## <a name="additional-references"></a>Referencias adicionales

-   [Información general sobre Virtualización de red de Hyper-V](hyperv-network-virtualization-overview-windows-server.md)

-   [Detalles técnicos de la Virtualización de red de Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)

-   [Redes definidas por software](../../software-defined-networking.md)