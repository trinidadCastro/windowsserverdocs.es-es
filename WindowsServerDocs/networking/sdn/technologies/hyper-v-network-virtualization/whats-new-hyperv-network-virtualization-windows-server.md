---
title: Novedades de virtualización de red de Hyper-V en Windows Server 2016
description: En este tema proporciona información sobre las nuevas características de virtualización de red de Hyper-V en Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: c87ccfba7b9ccc77646f58ade2853766524e67b1
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284040"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Novedades de virtualización de red de Hyper-V en Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe la funcionalidad de virtualización de red de Hyper-V (HNV) que es nuevos o han cambiado en Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Actualizaciones en HNV  
HNV ofrece compatibilidad mejorada en las áreas siguientes:  
  
|Característica/función|Nueva o mejorada|Descripción|  
|--------------------------|-------------------|---------------|  
|[Conmutador de Hyper-V programable](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nuevo|La directiva de HNV es programable a través de la controladora de red de Microsoft.|  
|[Soporte técnico de encapsulación VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nuevo|HNV ahora es compatible con la encapsulación de VXLAN.|  
|[Interoperabilidad de equilibrador de carga (SLB) de software](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nuevo|HNV está totalmente integrado con el equilibrador de carga de Software de Microsoft.|  
|[Compatible con encabezados de Ethernet IEEE](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Mejorada|Cumplir los estándares de Ethernet IEEE|  
  
### <a name="SDN"></a>Conmutador de Hyper-V programable  
HNV es un bloque de creación fundamental de la solución de redes definidas por Software (SDN) actualizada de Microsoft y está totalmente integrado en la pila de SDN.  
  
Nuevo controlador de red de Microsoft inserta las directivas HNV a un agente de Host que se ejecuta en cada host mediante vSwitch abierto protocolo de administración de base de datos (OVSDB) como la interfaz de SouthBound (SBI). El agente de Host almacena esta directiva mediante una personalización de la [esquema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) y programas en un motor de flujo de alto rendimiento en el conmutador de Hyper-V de las reglas de flujo complejos.  
  
El motor de flujo en el conmutador de Hyper-V es el mismo motor usado en Microsoft Azure&trade;, que se ha probado a gran escala en la nube pública de Microsoft Azure. Además, la pila de SDN toda copia a través de la controladora de red y el proveedor de recursos de red (detalles próximamente) es coherente con Microsoft Azure, por lo tanto la eficacia de la nube pública de Microsoft Azure en nuestra empresa y el servicio de hospedaje clientes de proveedores.  
  
> [!NOTE]  
> Para obtener más información sobre OVSDB, consulte [7047 RFC](https://www.rfc-editor.org/info/rfc7047).  
  
El conmutador de Hyper-V es compatible con ambas reglas de flujo con y sin estado según el motor de flujo simple 'coincide con acción' dentro de Microsoft.  
 
![Conmutador de Windows Server 2016 Hyper-V](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Soporte técnico de encapsulación VXLAN  
La red de área Local extensibles virtuales (VXLAN - [7348 RFC](https://www.rfc-editor.org/info/rfc7348)) protocolo se ha adoptado ampliamente en el mercado, con soporte técnico de proveedores como Cisco, Brocade, Dell, HP y otros usuarios. HNV ahora también es compatible con este esquema de encapsulación mediante el modo de distribución MAC a través de la controladora de red de Microsoft para las asignaciones de programa para inquilinos superposición red las direcciones IP (dirección de cliente o CA) a las direcciones IP de red físico subposición (proveedor Dirección o PA). NVGRE VXLAN tarea descarga se admiten tanto para mejorar el rendimiento a través de los controladores de terceros.  
  
### <a name="SLB"></a>Interoperabilidad de equilibrador de carga (SLB) de software  
Windows Server 2016 incluye un equilibrador de carga de software (SLB) con compatibilidad total para el tráfico de red virtual y la interacción perfecta con HNV. El SLB se implementa a través del motor de flujo de alto rendimiento en el plano de datos v-conmutador y controlado por la controladora de red para la dirección IP Virtual (VIP) / dinámica asignaciones de IP (DIP).  
  
### <a name="L2"></a>Compatible con encabezados de Ethernet IEEE  
HNV implementa encabezados de L2 Ethernet correctos para garantizar la interoperabilidad con dispositivos físicos y virtuales de terceros que dependen de los protocolos estándar del sector. Microsoft garantiza que todos los paquetes transmitidos tienen valores compatibles con todos los campos para garantizar esta interoperabilidad. Además, la compatibilidad con las tramas gigantes (MTU 1780 >) en la red física de L2 será necesarias para la cuenta para paquetes que se introdujo una sobrecarga por protocolos encapsulation (NVGRE, VXLAN) garantizando invitado mantener las máquinas virtuales conectadas a una red Virtual de HNV un MTU 1514.  
  
## <a name="see-also"></a>Vea también  
  
-   [Información general sobre Virtualización de red de Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Detalles técnicos de la Virtualización de red de Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Redes definidas por software](../../Software-Defined-Networking--SDN-.md)  
  