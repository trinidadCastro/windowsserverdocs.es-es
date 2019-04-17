---
title: Tecnologías SDN
description: Los temas de esta sección proporcionan información general e información técnica sobre las tecnologías de Software definido de red que se incluyen en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f842ac0d1a09106c1898374cf8dd1c7823d7dae
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="sdn-technologies"></a>Tecnologías SDN

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Los temas de esta sección proporcionan información general e información técnica sobre las tecnologías de Software definido de red que se incluyen en Windows Server 2016.  
  
> [!NOTE]  
> Para obtener documentación adicional Software definido de red, puedes usar las siguientes secciones de la biblioteca.  
>   
> - [Planear SDN](../plan/Plan-Software-Defined-Networking.md)
> - [Implementar SDN](../deploy/Deploy-Software-Defined-Networking.md)
> - [Administrar SDN](../manage/manage-sdn.md)
> - [Seguridad para SDN](../security/sdn-security-top.md)
> - [Solucionar problemas de SDN](../troubleshoot/Troubleshoot-Software-Defined-Networking.md)

Hay muchas tecnologías de funcionan juntos para crear soluciones de Software definido de redes (SDN) de Microsoft, incluidos los siguientes:  
  
-   **[Protocolo de enlace de borde & #40; BGP & #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)**  
  
    Cuando se configura en un Windows Server 2016 acceso servicio (RAS) puerta de enlace remoto, protocolo de puerta de enlace de borde (BGP) proporciona la capacidad para administrar el enrutamiento de tráfico de red entre redes de máquina virtual de su los de inquilinos y sus sitios remotos. BGP reduce la necesidad de configuración manual de ruta en los enrutadores, dado que es un protocolo de enrutamiento dinámico y aprende automáticamente rutas entre sitios que están conectados a través de conexiones de VPN de sitio a sitio.  
  
-   **[Introducción a Datacenter Firewall](../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)**  
  
    Firewall de centros de datos es un nuevo servicio que se incluye con Windows Server 2016. Es un nivel de red, la tupla de 5 (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino), el firewall con estado y para varios inquilinos. Cuando se implementa y ofrece como un servicio por el proveedor de servicios, los administradores de inquilino pueden instalar y configurar las directivas de firewall para ayudar a proteger sus redes virtuales de tráfico no deseado que se origina desde Internet y redes de intranet.  
  
  
-   **[Virtualización de la red de Hyper-V](../../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)**  
  
    Virtualización de red de Hyper-V (HNV) permite la virtualización de redes de cliente por encima de una infraestructura física de red compartida.  
  
- **[Interno de servicio DNS & #40; IDN & #41; para SDN](../../sdn/technologies/Idns-for-Sdn.md)**

    \(VMs\) hospedado máquinas virtuales y las aplicaciones requieren DNS para la comunicación entrante en sus propias redes con recursos externos en Internet. Con IDN, puedes proporcionar inquilinos con servicios de resolución de nombres DNS de su espacio de nombres aislado, local y de recursos de Internet.

-   **[Controlador de red](../../sdn/technologies/network-controller/Network-Controller.md)**  
  
    El controlador de red proporciona un punto programable centralizado de la automatización para administrar, configurar, supervisar y solucionar problemas de infraestructura de red físicas y virtuales en el centro de datos.  
  
-   **[Virtualización de la función de red](../../sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)**  
  
    Funciones de red que se realiza en dispositivos de hardware (como equilibradores de carga, firewalls, enrutadores, conmutadores y así sucesivamente) cada vez que se están virtualizadas como dispositivos virtuales.  
  
    Microsoft ha virtualizados redes, modificadores, puertas de enlace, NAT, equilibradores de carga y firewalls.  

-   **[Puerta de enlace RAS para SDN](../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)**
  
    Puerta de enlace de RAS es un protocolo de puerta de enlace de borde (BGP) basado en software y para varios inquilinos, compatible con enrutador en Windows Server 2016 está diseñada para proveedores de servicios de nube (CSP) y las empresas que hospedar varias redes virtuales inquilino usando la virtualización de Hyper-V red.  
      
- **[Remoto acceso directo a memoria & #40; RDMA & #41; y cambiar la agrupación incrustado & #40; Establecer & #41;](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)**  
  
    Puedes usar una NIC convergente para combinar el tráfico RDMA y Ethernet mediante un adaptador de red. La NIC convergente te permite usar un adaptador de red para la administración de almacenamiento remoto RDMA de acceso directo de memoria habilitados y el tráfico de inquilinos. Esto reduce los gastos de capital que están asociados a cada servidor en el centro de datos, porque se necesitan menos adaptadores de red para administrar los distintos tipos de tráfico por servidor.  
  
    CONJUNTO es una solución de equipo NIC que está integrada en el conmutador de Hyper-V Virtual. Permite la agrupación de hasta ocho NIC físicas en un solo equipo conjunto, lo que mejora la disponibilidad y proporciona la conmutación por error. En Windows Server 2016, puedes crear conjunto los equipos que están restringidos para el uso de bloque de mensajes de servidor (SMB) y RDMA.
  

-   **[Equilibrio de carga de software & #40; SLB & #41; para SDN](../../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)**  

    Proveedores de servicios de nube (CSP) y las empresas que implementan Software definido de redes (SDN) en Windows Server 2016 usar Equilibrio de carga de Software (SLB) para distribuir uniformemente inquilino y tráfico de red de cliente de inquilino entre los recursos de red virtual. La SLB de servidor de Windows permite varios servidores hospedar la misma carga de trabajo que escalabilidad y alta disponibilidad.
  
-   **[System Center](../../sdn/Sc-Tech-for-Sdn.md) ** puedes usar System Center 2016 Virtual Machine Manager (VMM) y Operations Manager para implementar y administrar la infraestructura SDN, incluidos los controladores de red, equilibradores de carga de software y puertas de enlace. También puedes usar VMM para definir y controlar las directivas de red virtual y vincular las directivas para las aplicaciones o cargas de trabajo de forma centralizada.
  
- **[Contenedores de Windows](../technologies/Containers/Container-networking-overview.md)**
    
    Windows Server contenedores son un método de virtualización de sistema operativo ligero usado para separar los servicios o aplicaciones de otros servicios que se ejecutan en el mismo host de contenedor. Para habilitar esto, cada contenedor tiene su propia vista del sistema operativo, procesos, sistema de archivos, registro y direcciones IP. Con Windows Server 2016, ahora puede conectar a redes virtuales contenedores de Windows Server. Función de contenedores de Windows de forma similar a máquinas virtuales en lo que respecta a la red. Cada contenedor tiene un adaptador de red virtual que está conectado a un conmutador virtual, que se reenvía el tráfico entrante y saliente. Para aplicar el aislamiento entre contenedores en el mismo host, se crea un compartimento de la red para cada Windows Server y el contenedor de Hyper-V en la que está instalado el adaptador de red para el contenedor. Contenedores de servidor de Windows usan un vNIC Host para adjuntar al conmutador virtual. Contenedores de Hyper-V se usa una sintéticos NIC de máquina virtual (no se expone a la máquina virtual de utilidad) para adjuntar el conmutador virtual.

