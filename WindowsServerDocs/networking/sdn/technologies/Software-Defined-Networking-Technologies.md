---
title: Tecnologías de SDN
description: Los temas de esta sección proporcionan información general e información técnica sobre las tecnologías de redes definidas por software que se incluyen en Windows Server 2016.
manager: grcusanz
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: anpaul
author: AnirbanPaul
ms.date: 02/14/2019
ms.openlocfilehash: 591a81c91dc444cfe48f0fa40142489b72142409
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952575"
---
# <a name="sdn-technologies"></a>Tecnologías de SDN

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Los temas de esta sección proporcionan información general e información técnica sobre las tecnologías de redes definidas por software que se incluyen en Windows Server 2016.

## <a name="network-controller"></a>[Controladora de red](network-controller/Network-Controller.md)

La controladora de red proporciona un punto de automatización programable y centralizado para administrar, configurar, supervisar y solucionar problemas de la infraestructura de red virtual y física en el centro de recursos. Con la controladora de red, puede automatizar la configuración de la infraestructura de red en lugar de realizar la configuración manual de los dispositivos y servicios de red.

La controladora de red es un servidor escalable y de alta disponibilidad y proporciona dos interfaces de programación de aplicaciones (API):

1. **SOUTHBOUND API** : permite que la controladora de red se comunique con la red.
2. **NORTHBOUND API** : permite comunicarse con el controlador de red.

Puede usar Windows PowerShell, la API de transferencia de estado representacional (REST) o una aplicación de administración para administrar la siguiente infraestructura de red física y virtual:

- MV Hyper-V y conmutadores virtuales
- Conmutadores de red físicos
- Enrutadores de red físicos
- Software de firewall
- Puertas de enlace de VPN, incluidas las puertas de enlace multiinquilino de servicio de acceso remoto (RAS)
- Equilibradores de carga

## <a name="hyper-v-network-virtualization"></a>[Virtualización de red de Hyper-V](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualización de red de Hyper-V (HNV) ayuda a abstraer las aplicaciones y cargas de trabajo de la red física mediante el uso de redes virtuales. Las redes virtuales proporcionan el aislamiento multiempresa necesario mientras se ejecutan en un tejido de red física compartida, con lo que aumenta la utilización de recursos. Para asegurarse de que puede llevar a cabo las inversiones existentes, puede configurar redes virtuales en el engranaje de red existente. Además, las redes virtuales son compatibles con las redes de área local virtual (VLAN).

## <a name="hyper-v-virtual-switch"></a>[Conmutador virtual de Hyper-V](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

El conmutador virtual de Hyper-V es un conmutador de red Ethernet de nivel 2 basado en software que está disponible en el administrador de Hyper-v después de haber instalado el rol de servidor Hyper-V. El conmutador incluye mediante programación funcionalidades extensibles y administradas para conectar máquinas virtuales a la red física y a redes virtuales. Además, el conmutador virtual de Hyper-V proporciona la aplicación de directivas para la seguridad, el aislamiento y los niveles de servicio.

También puede implementar el conmutador virtual de Hyper-V con switch Embedded Teaming (SET) y acceso directo a memoria remota (RDMA). Para obtener más información, vea la sección [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set) en este tema.

## <a name="internal-dns-service-idns-for-sdn"></a>[Servicio DNS interno (IDN) para SDN](Idns-for-Sdn.md)

Las máquinas virtuales (VM) y las aplicaciones hospedadas requieren que el DNS se comunique dentro de sus redes y con recursos externos en Internet. Con IDN, puede proporcionar inquilinos con servicios de resolución de nombres DNS para recursos de Internet y espacios de nombres locales aislados.

## <a name="network-function-virtualization"></a>[Virtualización de función de red](network-function-virtualization/Network-Function-Virtualization.md)

Los dispositivos de hardware, como equilibradores de carga, firewalls, enrutadores y conmutadores, se están convirtiendo cada vez más en dispositivos virtuales. Microsoft tiene redes virtualizadas, conmutadores, puertas de enlace, NAT, equilibradores de carga y firewalls. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red. Las aplicaciones virtuales están surgiendo rápidamente y crean un nuevo mercado. Continúan generando interés y ganar impulsos en las plataformas de virtualización y en los servicios en la nube.

Están disponibles las siguientes tecnologías de virtualización de función de red.

-   **Load balancer de software (SLB) y traducción de direcciones de red (NAT)**. Mejore el rendimiento admitiendo Direct Server Return en la que el tráfico de red de devolución puede omitir el multiplexor de equilibrio de carga. Para obtener más información, consulte [equilibrio de carga de software/(SLB/) para Sdn](network-function-virtualization/software-load-balancing-for-sdn.md).

-   **Firewall de Datacenter**. Proporcione listas de control de acceso (ACL) pormenorizadas, lo que le permite aplicar directivas de firewall en el nivel de interfaz de VM o en el nivel de subred. Para obtener más información, consulte [información general sobre el firewall del centro](network-function-virtualization/Datacenter-Firewall-Overview.md)de datos.

-   **Puerta de enlace ras para Sdn**. Enrute el tráfico de red entre la red física y los recursos de red de VM, independientemente de la ubicación. Puede enrutar el tráfico de red en la misma ubicación física o en muchas ubicaciones diferentes. Para obtener más información, consulte [puerta de enlace ras para Sdn](network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Acceso directo a memoria remota (RDMA) y Switch Embedded Teaming (SET)
En Windows Server 2016, puede habilitar RDMA en los adaptadores de red que están enlazados a un conmutador virtual de Hyper-V con o sin switch Embedded Teaming (SET). Esto le permite utilizar menos adaptadores de red cuando desee usar RDMA y establecer al mismo tiempo.

SET es una solución alternativa para la formación de equipos NIC que puede usar en entornos que incluyen Hyper-V y la pila de redes definidas por software (SDN) en Windows Server 2016. SET integra parte de la funcionalidad de formación de equipos NIC en el conmutador virtual de Hyper-V.

El conjunto le permite agrupar entre uno y ocho adaptadores de red Ethernet físicos en uno o varios adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.
Los adaptadores de red de miembros deben estar instalados en el mismo host físico de Hyper-V que se van a colocar en un equipo.

Además, puede usar comandos de Windows PowerShell para habilitar el protocolo de puente del centro de datos (DCB), crear un conmutador virtual de Hyper-V con una NIC virtual de RDMA (vNIC) y crear un conmutador virtual de Hyper-V con SET y RDMA VNIC. Para obtener más información, vea [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="border-gateway-protocol-bgp"></a>[Protocolo de puerta de enlace de borde (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)

Protocolo de puerta de enlace de borde (BGP) es un protocolo de enrutamiento dinámico que aprende automáticamente las rutas entre sitios que usan conexiones VPN de sitio a sitio. Por lo tanto, BGP reduce la configuración manual de los enrutadores.   Al configurar la puerta de enlace de RAS, BGP le permite administrar el enrutamiento del tráfico de red entre las redes de máquinas virtuales de los inquilinos y los sitios remotos.

## <a name="software-load-balancing-slb-for-sdn"></a>[Equilibrio de carga de software (SLB) para SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
Los proveedores de servicios en la nube (CSP) y las empresas que implementan SDN pueden usar el equilibrio de carga de software (SLB) para distribuir uniformemente el tráfico de red del inquilino y del cliente del inquilino entre los recursos de red virtual. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.

## <a name="windows-server-containers"></a>[Contenedores de Windows Server](Containers/Container-networking-overview.md)

Los contenedores de Windows Server son un método ligero de virtualización del sistema operativo que separa aplicaciones o servicios de otros servicios que se ejecutan en el mismo host de contenedor. Cada contenedor tiene su propio sistema operativo, procesos, sistema de archivos, registro y direcciones IP, que se pueden conectar a redes virtuales.

## <a name="system-center"></a>System Center

Implemente y administre la infraestructura de SDN con [Administración de máquinas virtuales (VMM)](https://docs.microsoft.com/system-center/vmm/) y [Operations Manager](https://docs.microsoft.com/system-center/scom/). Con VMM, aprovisiona y administra los recursos necesarios para crear e implementar máquinas virtuales y servicios en nubes privadas.  Con Operations Manager, supervisa servicios, dispositivos y operaciones en toda la empresa para identificar problemas que requieren una acción inmediata.


---