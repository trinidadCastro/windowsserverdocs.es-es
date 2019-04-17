---
title: Novedades en la virtualización de la red de Hyper-V en Windows Server 2016
description: Este tema proporciona información sobre las nuevas características de virtualización de la red de Hyper-V en Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: 0954768944e44848debfbb7fb752a13ca47031c2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Novedades en la virtualización de la red de Hyper-V en Windows Server 2016

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema se describe la funcionalidad de virtualización de red de Hyper-V (HNV) que es nuevo o cambiado en Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Actualizaciones en HNV  
HNV ofrece compatibilidad mejorada en las siguientes áreas:  
  
|Características y funcionalidades|Nuevas o mejoradas|Descripción|  
|--------------------------|-------------------|---------------|  
|[Conmutador de Hyper-V programable](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nuevo|Directiva HNV es programable a través del controlador de red de Microsoft.|  
|[Soporte técnico de encapsulación VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nuevo|HNV ahora admite encapsulación VXLAN.|  
|[Interoperabilidad del software carga equilibrado (SLB)](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nuevo|HNV está totalmente integrado con el equilibrado de carga de Software de Microsoft.|  
|[Encabezados de IEEE Ethernet compatible con](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Mejorado|Compatible con estándares IEEE Ethernet|  
  
### <a name="SDN"></a>Conmutador de Hyper-V programable  
HNV es una unidad fundamental de la creación de soluciones de Software definido de redes (SDN) actualizada de Microsoft y está totalmente integrado en la pila SDN.  
  
Nuevo controlador de red de Microsoft inserta directivas HNV hasta un agente de Host que se ejecuta en cada host con abrir vSwitch protocolo de administración de base de datos (OVSDB) como interfaz SouthBound (SBI). El agente de Host almacena esta directiva con una personalización de la [esquema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) y programas de reglas de flujo complejo en un motor de flujo de alto rendimiento en el conmutador de Hyper-V.  
  
El motor de flujo en el interruptor de Hyper-V es el mismo motor que se usan en Microsoft Azure&trade;, que se ha comprobado a gran escala en la nube pública de Microsoft Azure. Además, el stack SDN arriba a través del controlador de red y el proveedor de recursos de red (más detalles próximamente) es coherente con Microsoft Azure, lo que proporciona la potencia de la nube pública de Microsoft Azure a nuestra empresa y el hospedaje de los clientes de proveedor de servicio.  
  
> [!NOTE]  
> Para obtener más información sobre OVSDB, consulta [RFC 7047](http://www.rfc-editor.org/info/rfc7047).  
  
El conmutador de Hyper-V es compatible con ambas reglas de flujo con y sin estado en función de motor de flujo simple 'coincide con acción' dentro de Microsoft.  
 
![Conmutador de Windows Server 2016 Hyper-V](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Soporte técnico de encapsulación VXLAN  
Red de área Local eXtensible Virtual (VXLAN - [RFC 7348](http://www.rfc-editor.org/info/rfc7348)) se ha adoptado ampliamente protocolo en el mercado, con el soporte técnico de los proveedores como Cisco, Brocade, Dell, HP y otros. HNV ahora también es compatible con este esquema de encapsulación con el modo de distribución de MAC mediante el controlador de red de Microsoft a las asignaciones de programa inquilino superposición red las direcciones IP (dirección de cliente o entidad de certificación) para las direcciones IP a red subposición físico (dirección de proveedor o PA). NVGRE VXLAN tarea de descarga se admiten tanto para mejorar el rendimiento mediante controladores de terceros.  
  
### <a name="SLB"></a>Interoperabilidad del software carga equilibrado (SLB)  
Windows Server 2016 incluye un equilibrado de carga de software (SLB) con soporte completo para el tráfico de red virtual e interacción perfecta con HNV. Se implementa mediante el motor de flujo de alto rendimiento en el modificador de v plano datos y controlada por el controlador de red para IP Virtual (VIP) / dinámico el SLB asignaciones IP (DIP).  
  
### <a name="L2"></a>Encabezados de IEEE Ethernet compatible con  
HNV implementa los encabezados de Ethernet L2 correctos para garantizar la interoperabilidad con dispositivos físicos y virtuales de terceros que dependen de los protocolos estándar del sector. Microsoft garantiza que todos los paquetes transmitidos valores compatibles con todos los campos para garantizar la interoperabilidad. Además, soporte técnico para fotogramas gigante (MTU 1780 >) en la red L2 física será necesario para tener en cuenta para paquete sobrecarga introducido por protocolos de encapsulación (NVGRE, VXLAN) mientras se asegura de invitado Virtual de los equipos conectados a una red Virtual de HNV mantener una MTU 1514.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Introducción a la virtualización de Hyper-V red](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Detalles técnicos de virtualización de Hyper-V en red](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Software definido redes](../../Software-Defined-Networking--SDN-.md)  
  