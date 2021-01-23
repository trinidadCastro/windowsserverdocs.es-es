---
title: Alta disponibilidad de la controladora de red
description: Puede usar este tema para obtener información sobre la alta disponibilidad de la controladora de red para redes definidas por software (SDN) en Windows Server 2019 y 2016.
manager: grcusanz
ms.topic: how-to
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: anpaul
author: AnirbanPaul
ms.date: 12/08/2020
ms.openlocfilehash: 5990591e087561ff8e327cca8db489fda501b86c
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716401"
---
# <a name="network-controller-high-availability"></a>Alta disponibilidad de la controladora de red

>Se aplica a: Windows Server 2019, Windows Server 2016

Puede usar este tema para obtener información sobre la configuración de escalabilidad y alta disponibilidad de la controladora de red para redes definidas por software \( Sdn \) .

Al implementar SDN en el centro de información, puede usar la controladora de red para implementar, supervisar y administrar de forma centralizada muchos elementos de red, como puertas de enlace RAS, equilibradores de carga de software, directivas de redes virtuales para la comunicación de inquilinos, directivas de Firewall de centro de seguridad, QoS de calidad de servicio \( \) para directivas de Sdn, directivas de red híbrida, etc

Dado que la controladora de red es la piedra angular de la administración de SDN, es fundamental que las implementaciones de la controladora de red proporcionen alta disponibilidad y la posibilidad de escalar vertical u horizontalmente los nodos de la controladora de red con las necesidades del centro de seguridad.

Aunque puede implementar la controladora de red como un clúster de máquina única, para la alta disponibilidad y la conmutación por error, debe implementar la controladora de red en un clúster de varias máquinas con un mínimo de tres equipos.

>[!NOTE]
>Puede implementar la controladora de red en equipos servidor o en máquinas virtuales de máquinas virtuales \( \) que ejecutan Windows Server 2016 Datacenter Edition. Si implementa el controlador de red en las máquinas virtuales, las máquinas virtuales deben ejecutarse en hosts de Hyper-V que también ejecuten Datacenter Edition. La controladora de red no está disponible en Windows Server 2016 Standard Edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controladora de red como una aplicación Service Fabric

Para lograr una alta disponibilidad y escalabilidad, Controladora de red se basa en Service Fabric. Service Fabric proporciona una plataforma de sistemas distribuidos para compilar aplicaciones escalables, confiables y fáciles de administrar.

Como plataforma, Service Fabric proporciona la funcionalidad necesaria para crear un sistema distribuido escalable. Proporciona hospedaje de servicio en varias instancias de sistema operativo, sincronización de información de estado entre instancias, elección de una directriz, detección de errores, equilibrio de carga, etc.

>[!NOTE]
>Para obtener información sobre Service Fabric en Azure, consulte [información general de azure Service fabric](/azure/service-fabric/service-fabric-overview).

Al implementar la controladora de red en varios equipos, la controladora de red se ejecuta como una sola aplicación de Service Fabric en un clúster de Service Fabric. Puede crear un clúster de Service Fabric conectando un conjunto de instancias del sistema operativo.

La aplicación de controladora de red se compone de varios servicios Service Fabric con estado. Cada servicio es responsable de una función de red, como la administración de redes físicas, la administración de redes virtuales, la administración de firewall o la administración de puertas de enlace.

Cada servicio de Service Fabric tiene una réplica principal y dos réplicas secundarias. La réplica de servicio principal procesa las solicitudes, mientras que las dos réplicas de servicio secundarias proporcionan alta disponibilidad en las circunstancias en las que la réplica principal está deshabilitada o no está disponible por algún motivo.

En la ilustración siguiente se muestra una controladora de red Service Fabric clúster con cinco equipos. Se distribuyen cuatro servicios entre las cinco máquinas: servicio de firewall, servicio de puerta de enlace, servicio SLB de equilibrio de carga de software \( \) y servicio de red virtual de red virtual \( \) .  Cada uno de los cuatro servicios incluye una réplica de servicio principal y dos réplicas de servicio secundarias.

![Clúster de Service Fabric de controladora de red](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Ventajas del uso de Service Fabric

A continuación se enumeran las ventajas principales del uso de Service Fabric para los clústeres de controladora de red.

### <a name="high-availability-and-scalability"></a>Alta disponibilidad y escalabilidad

Dado que el controlador de red es el núcleo de una red de centro de recursos, debe ser resistente a los errores y ser lo suficientemente escalable como para permitir cambios ágiles en las redes del centro de recursos a lo largo del tiempo. Las siguientes características proporcionan estas capacidades:

- **Conmutación por error rápida**. Service Fabric proporciona una conmutación por error extremadamente rápida. Siempre están disponibles varias réplicas de servicio secundarias activas. Si una instancia de sistema operativo deja de estar disponible debido a un error de hardware, una de las réplicas secundarias se promueve inmediatamente a la réplica principal.
- **Agilidad de la escala**. Puede escalar con facilidad y rapidez los servicios de confianza desde varias instancias hasta miles de instancias y, a continuación, volver a realizar una copia de seguridad en algunas instancias, en función de sus necesidades de recursos.

### <a name="persistent-storage"></a>Almacenamiento persistente

La aplicación de controladora de red tiene grandes requisitos de almacenamiento para su configuración y estado. La aplicación también debe ser utilizable en las interrupciones planeadas y no planeadas. Para este propósito, Service Fabric proporciona un Key-Value de almacenamiento de \( KVS \) que es un almacén transaccional y replicado.

### <a name="modularity"></a>Modularidad

El controlador de red está diseñado con una arquitectura modular, donde cada uno de los servicios de red, como el servicio de redes virtuales y el servicio de firewall, están integrados \- como servicios individuales.

Esta arquitectura de aplicación proporciona las siguientes ventajas.

1. La modularidad de la controladora de red permite el desarrollo independiente de cada uno de los servicios admitidos, a medida que evolucionen las necesidades. Por ejemplo, el servicio de equilibrio de carga de software se puede actualizar sin que afecte a ninguno de los demás servicios o al funcionamiento normal de la controladora de red.
2. La modularidad de la controladora de red permite la adición de nuevos servicios, a medida que la red evolucione. Se pueden agregar nuevos servicios a la controladora de red sin que ello afecte a los servicios existentes.

>[!NOTE]
>En Windows Server 2016, no se admite la adición de servicios de terceros a la controladora de red.

Service Fabric modularidad utiliza esquemas de modelo de servicio para maximizar la facilidad de desarrollo, implementación y mantenimiento de una aplicación.

## <a name="network-controller-deployment-options"></a>Opciones de implementación de la controladora de red

Para implementar la controladora de red mediante System Center Virtual Machine Manager \( VMM \) , consulte [configuración de una controladora de red de Sdn en el tejido de VMM](/system-center/vmm/sdn-controller).

Para implementar la controladora de red mediante scripts, consulte [implementación de una infraestructura de red definida por software mediante scripts](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implementar la controladora de red con Windows PowerShell, consulte implementación de la [controladora de red con Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md) .

Para obtener más información acerca de la controladora de red, consulte [controladora de red](Network-Controller.md).
