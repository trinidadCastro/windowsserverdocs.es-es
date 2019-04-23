---
title: Tecnologías SDN
description: Los temas de esta sección proporcionan información general y obtener información técnica acerca de las tecnologías de redes definidas por Software que se incluyen en Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: acf3e1dc3e5a229c525ba7cad23819640c0d5261
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891166"
---
# <a name="sdn-technologies"></a>Tecnologías SDN

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Los temas de esta sección proporcionan información general y obtener información técnica acerca de las tecnologías de redes definidas por Software que se incluyen en Windows Server 2016.  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[Controladora de red](network-controller/Network-Controller.md)

La controladora de red proporciona un punto centralizado programable de automatización para administrar, configurar, supervisar y solucionar problemas tanto virtual y la infraestructura de red física del centro de datos. Con la controladora de red, puede automatizar la configuración de la infraestructura de red en lugar de realizar la configuración manual de los dispositivos de red y servicios. 

La controladora de red es un servidor escalable y altamente disponible y proporciona dos interfaces de programación de aplicaciones (API):

1. **La API southbound** : permite que la controladora de red para comunicarse con la red.
2. **La API northbound** – le permite comunicarse con la controladora de red.

Puede usar Windows PowerShell, la API de Representational State Transfer (REST) o una aplicación de administración para administrar la infraestructura de red físicos y virtuales siguientes:

- MV Hyper-V y conmutadores virtuales 
- Conmutadores de red físicos 
- Enrutadores de red físicos 
- Software de firewall 
- Puertas de enlace de VPN, incluidas las puertas de enlace de servicio de acceso remoto (RAS) para varios inquilinos 
- Equilibradores de carga 
  

  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualización de red de Hyper-V](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualización de red de Hyper-V (HNV) le ayuda a extraer sus aplicaciones y cargas de trabajo de la red física mediante el uso de redes virtuales. Las redes virtuales proporcionan el aislamiento multiempresa necesario mientras se ejecutan en un tejido de red física compartida, con lo que aumenta la utilización de recursos. Para asegurarse de que puede llevar adelante las inversiones existentes, puede configurar las redes virtuales en el equipo de red existente. Además, las redes virtuales son compatibles con redes de área Local virtual (VLAN).   
  
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Conmutador Virtual de Hyper-V](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

El conmutador Virtual de Hyper-V es un basada en software de capa 2 Ethernet conmutador de red que está disponible en el Administrador de Hyper-V después de haber instalado el rol de Hyper-V server. El conmutador incluye mediante programación funcionalidades extensibles y administradas para conectar máquinas virtuales a la red física y a redes virtuales. Asimismo, el conmutador Virtual de Hyper-V proporciona cumplimiento de directivas de seguridad, aislamiento y los niveles de servicio.
  
También puede implementar el conmutador Virtual de Hyper-V con Switch Embedded Teaming (SET) y acceso de memoria directa remota (RDMA). Para obtener más información, consulte la sección [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](#bkmk_rdma) en este tema.  

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[Servicio DNS interno (iDNS) para SDN](Idns-for-Sdn.md)

Las máquinas virtuales hospedadas (VM) y las aplicaciones requieren DNS se comuniquen en sus redes y con recursos externos en Internet. Con iDNS, puede proporcionar inquilinos con servicios de resolución de nombres DNS para el espacio de nombres local, aislado y los recursos de Internet. 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualización de red de función](network-function-virtualization/Network-Function-Virtualization.md)

Dispositivos de hardware, como los equilibradores de carga, firewalls, enrutadores y conmutadores son cada vez más aplicaciones virtuales. Microsoft ha virtualizado redes, conmutadores, las puertas de enlace, NAT, equilibradores de carga y firewalls. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red. Dispositivos virtuales rápidamente se emergentes y creación de un mercado completamente nuevo. Se seguirán generando interés y obtener momentum en ambas plataformas de virtualización y servicios en la nube. 
  
Están disponibles las siguientes tecnologías de virtualización de función de la red.  
  
-   **Equilibrador de carga de software (SLB) y direcciones de red NAT (traducción)**. Mejorar el rendimiento al admitir Direct Server Return en la que el tráfico de red de valor devuelto puede omitir el multiplexor de equilibrio de carga. Para obtener más información, consulte [/(SLB/) equilibrio de carga de Software para SDN](network-function-virtualization/software-load-balancing-for-sdn.md).
  
-   **Datacenter Firewall**. Proporcionar listas de control de acceso granular (ACL), lo que le permite aplicar directivas de firewall en el nivel de interfaz de la máquina virtual o el nivel de subred. Para obtener más información, consulte [información general de Datacenter Firewall](network-function-virtualization/Datacenter-Firewall-Overview.md).
  
-   **Puerta de enlace RAS para SDN**. Enrutar el tráfico de red entre la red física y los recursos de red de máquina virtual, independientemente de la ubicación. Puede enrutar el tráfico de red en la misma ubicación física o en muchas ubicaciones diferentes. Para obtener más información, consulte [puerta de enlace RAS para SDN](network-function-virtualization/RAS-Gateway-for-SDN.md).

  
## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-sethttpsdocsmicrosoftcomwindows-servervirtualizationhyper-v-virtual-switchrdma-and-switch-embedded-teaming"></a>[Acceso de memoria directa remota (RDMA) y Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)  
En Windows Server 2016, puede habilitar RDMA en los adaptadores de red que están enlazados a un conmutador Virtual de Hyper-V con o sin Switch Embedded Teaming (SET). Esto le permite usar menos adaptadores de red al que desea usar RDMA y un conjunto al mismo tiempo.  
  
CONJUNTO es una solución alternativa de formación de equipos NIC que puede usar en entornos que incluyen Hyper-V y la pila de redes definidas por Software (SDN) en Windows Server 2016. CONJUNTO de algunas de las funcionalidades de formación de equipos NIC integra en el conmutador Virtual de Hyper-V.  
  
CONJUNTO permite agrupar entre uno y ocho Ethernet adaptadores de red físicos en uno o más adaptadores de red virtual basada en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.  
CONJUNTO de adaptadores de red de miembro deben instalarse en el mismo host físico de Hyper-V que se colocarán en un equipo.  
  
Además, puede usar comandos de Windows PowerShell para habilitar el protocolo de puente del centro de datos (DCB), cree un conmutador Virtual de Hyper-V con una NIC virtual (vNIC) de RDMA y crear un conmutador Virtual de Hyper-V con VNIC RDMA y.  

  

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[Protocolo de puerta de enlace de borde (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
Border Gateway Protocol (BGP) es un protocolo de enrutamiento dinámico que aprende automáticamente las rutas entre sitios que usan conexiones de VPN de sitio a sitio. Por lo tanto, reduce la configuración manual de los enrutadores de BGP.   Al configurar la puerta de enlace RAS, BGP permite administrar el enrutamiento de tráfico de red entre redes de máquinas virtuales y los sitios remotos de los inquilinos.  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[Software equilibrio de carga (SLB) para SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
Proveedores de servicios en la nube (CSP) y las empresas que implementan SDN pueden usar Equilibrio de carga de Software (SLB) para distribuir uniformemente tráfico de red de cliente de inquilino entre recursos de red virtual y del inquilino. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad. 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Contenedores de Windows Server](Containers/Container-networking-overview.md)

Contenedores de Windows Server son un método de virtualización del sistema operativo ligero separar las aplicaciones o servicios de otros servicios que se ejecutan en el mismo host de contenedor. Cada contenedor tiene su propio sistema operativo, procesos, sistema de archivos, el registro y las direcciones IP, que puede conectarse a redes virtuales. 


## <a name="system-center"></a>System Center  
Implementar y administrar la infraestructura de SDN con [administración de la máquina Virtual (VMM)](https://docs.microsoft.com/system-center/vmm/) y [Operations Manager](https://docs.microsoft.com/system-center/scom/). Con VMM, aprovisionar y administrar los recursos necesarios para crear e implementar máquinas virtuales y servicios en nubes privadas.  Con Operations Manager, supervisar servicios, dispositivos y operaciones en toda la empresa a identificar problemas de una acción inmediata. 


---