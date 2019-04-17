---
title: Alta disponibilidad del controlador de red
description: Puedes usar este tema para obtener información sobre alta disponibilidad de controlador de red para Software definido de redes (SDN) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f260f3e4d8ca5fcd998824327478c2fbe3c81875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-high-availability"></a>Alta disponibilidad del controlador de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre la alta disponibilidad de controlador de red y la configuración de escalabilidad de \(SDN\) Software definido de red.

Cuando implementas SDN en el centro de datos, puedes usar el controlador de red para implementar, supervisar y administrar muchos elementos de la red, incluidas las puertas de enlace de RAS, equilibradores de carga de Software, las directivas de redes virtuales para comunicación de inquilinos, las directivas de Firewall del centro de datos, calidad de servicio \(QoS\) SDN directivas, las directivas de redes híbrido, y mucho más de forma centralizada.

Dado que el controlador de red es el aspecto fundamental de la administración de SDN, es fundamental para las implementaciones de controlador de red proporcionar alta disponibilidad y la capacidad para TI a escala fácilmente hacia arriba o hacia abajo de nodos de controlador de red con tus necesidades de centros de datos.

Aunque se puede implementar el controlador de red como un clúster solo equipo, para conmutación por error y alta disponibilidad debe implementar el controlador de red en un clúster de varios equipos con un mínimo de tres máquinas.

>[!NOTE]
>Puedes implementar el controlador de red en ambos equipos de servidor o en \(VMs\) máquinas virtuales que ejecutan Windows Server 2016 Datacenter edition. Si implementas el controlador de red en máquinas virtuales, deben ejecutar las máquinas virtuales en hosts de Hyper-V que también ejecuten Datacenter edition. Controlador de red no está disponible en Windows Server 2016 Standard edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controlador de red como una aplicación de Service Fabric

Para lograr escalabilidad y alta disponibilidad, el controlador de red se basa en Service Fabric. Service Fabric proporciona una plataforma de sistemas distribuidos para generar escalable y confiable y fácil de administrar aplicaciones.

Como una plataforma, Service Fabric proporciona funciones que se necesita para crear un sistema distribuido escalable. Proporciona hospedaje del servicio en varias instancias de sistema operativo, sincronizar la información de estado entre instancias, elige un líder, detección de un error, equilibrio de carga y mucho más.

>[!NOTE]
>Para obtener información sobre Service Fabric en Azure, consulta [Introducción a Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Cuando implementas un controlador de red en varios equipos, el controlador de red se ejecuta como una sola aplicación Service Fabric en un clúster Service Fabric. Puede formar un clúster Service Fabric conectando un conjunto de instancias de sistema operativo.

La aplicación de controlador de red se compone de varios servicios Service Fabric con estado. Cada servicio es responsable de una función de la red, como la administración de red física, administración de red virtual, administración del firewall o administración de puerta de enlace. 

Cada servicio Service Fabric tiene una réplica principal y dos réplicas secundarias. La réplica del servicio principal procesa las solicitudes, mientras que las dos réplicas servicio secundario proporcionan alta disponibilidad en situaciones donde la réplica principal está desactivada o no está disponible por algún motivo.

La ilustración siguiente muestra un clúster Service Fabric de controlador de red con cinco máquinas. Cuatro servicios se distribuyen a través de las cinco máquinas: servicio de Firewall, servicio puerta de enlace, el servicio de equilibrio de carga de Software \(SLB\) y servicio de red virtual \(Vnet\).  Cada uno de los cuatro servicios incluye una réplica de servicio principal y dos réplicas servicio secundario.

![Clúster de controlador Service Fabric de red](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Ventajas de usar Service Fabric

Los siguientes son las principales ventajas de usar Service Fabric clústeres de controlador de red.

### <a name="high-availability-and-scalability"></a>Escalabilidad y alta disponibilidad

Dado que el controlador de red es el núcleo de una red de centros de datos, se debe tanto resistente a errores y ser lo suficientemente escalable permitir ágiles cambios en las redes datacenter con el tiempo. Las siguientes características proporcionan estas capacidades: 

- **Rápido conmutación por error**. Service Fabric proporciona extremadamente rápida conmutación por error. Múltiples réplicas hot servicio secundarios siempre están disponibles. Si una instancia del sistema operativo no está disponible debido a un error de hardware, una de las réplicas secundarias se promueve inmediatamente a la réplica principal. 
- **La agilidad de escala**. Rapidez y facilidad puede escalar estos servicios confiables de instancias de unos hasta miles de instancias y, después, bajar en algunos casos, según tus necesidades de recursos. 

### <a name="persistent-storage"></a>Almacenamiento persistente

La aplicación de controlador de red tiene requisitos de almacenamiento grandes para la configuración y el estado. La aplicación también debe ser utilizable en planificado e interrupciones. Para este propósito, Service Fabric proporciona un \(KVS\) almacén de pares clave-valor que es un almacén replicado, transaccional y persistente.

### <a name="modularity"></a>Modularidad

Controlador de red está diseñado con una arquitectura modular con cada uno de los servicios de red, como el servicio de redes virtuales y el servicio de firewall, built\ en como servicios individuales. 

Esta arquitectura de aplicación proporciona las siguientes ventajas.

1. Modularidad permite desarrollo independiente de cada uno de los servicios admitidos, como el controlador de red necesita evolucionando. Por ejemplo, el servicio de equilibrio de carga de Software puede actualizarse sin afectar a cualquiera de los demás servicios o el funcionamiento normal del controlador de red.
2. Modularidad de controlador de red permite la adición de nuevos servicios, a medida que evoluciona de la red. Nuevos servicios pueden agregarse al controlador de red sin afectar a los servicios existentes.

>[!NOTE]
>En Windows Server 2016, no se admite la adición de servicios de terceros para el controlador de red.

Modularidad Service Fabric utiliza los esquemas de modelo de servicio para maximizar la facilidad de desarrollo, implementación y mantenimiento de una aplicación.

## <a name="network-controller-deployment-options"></a>Opciones de implementación del controlador de red

Para implementar el controlador de red mediante el uso de System Center Virtual Machine Manager \(VMM\), consulta [configurar un controlador de red SDN en la estructura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implementar el controlador de red mediante scripts, consulta [implementar un Software definido red infraestructura usar Scripts de](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implementar el controlador de red mediante Windows PowerShell, consulta [implementar controladores de red mediante Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Para obtener más información sobre el controlador de red, consulta [controlador de red](Network-Controller.md).