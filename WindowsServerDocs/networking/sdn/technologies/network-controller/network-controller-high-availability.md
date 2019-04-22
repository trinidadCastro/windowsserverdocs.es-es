---
title: Alta disponibilidad de la controladora de red
description: Puede utilizar este tema para obtener información sobre la alta disponibilidad de la controladora de red para definidas por Software Networking (SDN) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbd3ae9f4c1f1fc3035fae9ace880046312df2f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813356"
---
# <a name="network-controller-high-availability"></a>Alta disponibilidad de la controladora de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre los configuración alta de disponibilidad y escalabilidad de controladora de red para redes definidas por Software \(SDN\).

Cuando se implementa SDN en el centro de datos, se puede usar controladora de red para implementar, supervisar y administrar muchos elementos de red, incluidas las puertas de enlace RAS, equilibradores de carga de Software, que las directivas de red virtuales para la comunicación del inquilino, de forma centralizada Datacenter Firewall las directivas de calidad de servicio \(QoS\) para directivas SDN, directivas de red híbrida y mucho más.

Dado que el controlador de red es la piedra angular de la administración de SDN, es fundamental para las implementaciones de controladora de red proporcionar alta disponibilidad y la capacidad para usted fácilmente escalar o reducir verticalmente los nodos de controladora de red con sus necesidades de centro de datos.

Aunque puede implementar la controladora de red como un clúster de máquina única, para alta disponibilidad y conmutación por error debe implementar controladora de red en un clúster de varios equipos con un mínimo de tres equipos.

>[!NOTE]
>Puede implementar la controladora de red en ambos equipos de servidor o en máquinas virtuales \(máquinas virtuales\) que ejecutan Windows Server 2016 Datacenter edition. Si implementa la controladora de red en máquinas virtuales, deben ejecutar las máquinas virtuales en hosts de Hyper-V que también estén ejecutando Datacenter edition. Controladora de red no está disponible en Windows Server 2016 Standard edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controladora de red como una aplicación de Service Fabric

Para lograr alta disponibilidad y escalabilidad, la controladora de red se basa en Service Fabric. Service Fabric proporciona una plataforma de sistemas distribuidos para compilar escalable, confiable y fácil de administrar las aplicaciones.

Como plataforma, Service Fabric proporciona la funcionalidad necesaria para la creación de un sistema distribuido escalable. Proporciona hospedaje de servicios en varias instancias de sistema operativo, sincronización de la información de estado entre las instancias, elección de líder, detección de errores, equilibrio de carga y mucho más.

>[!NOTE]
>Para obtener información acerca de Service Fabric en Azure, consulte [información general de Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Al implementar la controladora de red en varios equipos, controladora de red se ejecuta como una sola aplicación de Service Fabric en un clúster de Service Fabric. Puede formar un clúster de Service Fabric mediante la conexión de un conjunto de instancias de sistema operativo.

La aplicación controladora de red se compone de varios servicios de Service Fabric con estado. Cada servicio es responsable de una función de la red, como la administración de red física, administración de red virtual, administración del firewall o la administración de puerta de enlace. 

Cada servicio de Service Fabric tiene una réplica principal y dos réplicas secundarias. La réplica principal del servicio procesa las solicitudes, mientras que las dos réplicas de servicio secundario proporcionan alta disponibilidad en circunstancias donde la réplica principal está deshabilitado o no está disponible por alguna razón.

La siguiente ilustración muestra un clúster de Service Fabric de controladora de red con cinco máquinas. Cuatro servicios se distribuyen entre las cinco máquinas: Equilibrio de carga de servicio, el servicio de puerta de enlace, Software de Firewall \(SLB\) red virtual y servicio \(Vnet\) service.  Cada uno de los cuatro servicios incluye una réplica de servicio principal y dos réplicas de servicio secundario.

![Clúster de Service Fabric de controlador de red](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Ventajas del uso de Service Fabric

Estos son las principales ventajas de utilizar Service Fabric para clústeres de controladora de red.

### <a name="high-availability-and-scalability"></a>Alta disponibilidad y escalabilidad

Dado que el controlador de red es el núcleo de una red de centros de datos, debe tanto sea resistente a errores y ser lo suficientemente escalable para permitir cambios de agile en redes de centro de datos con el tiempo. Las siguientes características de proporcionan estas capacidades: 

- **Conmutación por error rápida**. Service Fabric ofrece conmutación por error muy rápida. Varias réplicas de servicio secundaria activa siempre están disponibles. Si una instancia de sistema operativo no está disponible debido a un error de hardware, una de las réplicas secundarias se promueve inmediatamente a la réplica principal. 
- **Agilidad de escala**. Fácil y rápidamente puede escalar de estos servicios confiables de unas pocas instancias hasta miles de instancias y, a continuación, volver a reducirlos verticalmente algunas instancias, según sus necesidades de recursos. 

### <a name="persistent-storage"></a>Almacenamiento persistente

La aplicación controladora de red tiene requisitos de almacenamiento de gran tamaño para la configuración y el estado. La aplicación también debe ser utilizable en interrupciones planificadas e imprevistas. Para ello, Service Fabric proporciona un Store pares clave-valor \(KVS\) que es un almacén replicado, transaccional y persistente.

### <a name="modularity"></a>Modularidad

Controladora de red está diseñada con una arquitectura modular, con cada uno de los servicios de red, como el servicio de redes virtuales y el servicio de firewall, compilado\-en como los servicios individuales. 

Esta arquitectura de aplicación proporciona las siguientes ventajas.

1. Controladora de red modularidad permite el desarrollo independiente de cada uno de los servicios admitidos, como necesidades evolucionan. Por ejemplo, el servicio de equilibrio de carga de Software puede actualizarse sin que afecte a cualquiera de los otros servicios o el funcionamiento normal de la controladora de red.
2. Modularidad de la controladora de red permite la adición de nuevos servicios a medida que evoluciona la red. Pueden agregarse nuevos servicios a la controladora de red sin que afecte a los servicios existentes.

>[!NOTE]
>En Windows Server 2016, no se admite la adición de servicios de terceros para la controladora de red.

Modularidad de Service Fabric usa esquemas de modelo de servicio para maximizar la facilidad de desarrollo, implementación y mantenimiento de una aplicación.

## <a name="network-controller-deployment-options"></a>Opciones de implementación del controlador de red

Para implementar la controladora de red mediante System Center Virtual Machine Manager \(VMM\), consulte [configurar un controlador de red SDN en el tejido de VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implementar la controladora de red con las secuencias de comandos, consulte [implementar un Software define infraestructura usando los Scripts de red](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implementar la controladora de red mediante Windows PowerShell, consulte [implementar controladora de red mediante Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Para obtener más información acerca de la controladora de red, consulte [controladora de red](Network-Controller.md).