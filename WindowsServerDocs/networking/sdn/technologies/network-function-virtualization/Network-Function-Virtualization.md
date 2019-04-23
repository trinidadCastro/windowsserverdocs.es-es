---
title: Virtualización de función de red
description: Puede utilizar este tema para obtener información acerca de la virtualización de función de red, lo que permite implementar aplicaciones virtuales de red como Datacenter Firewall, puerta de enlace RAS multiempresa y equilibrio de carga de Software (SLB) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59474a13d1cbce6a607f025caf3f6c1b839c7eed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884556"
---
# <a name="network-function-virtualization"></a>Virtualización de función de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca de la virtualización de función de red, lo que permite implementar aplicaciones virtuales de red como el Firewall de centro de datos, para varios inquilinos puerta de enlace RAS y equilibrio de carga de Software \(SLB\) multiplexor \(MUX\).
  
>[!NOTE]  
>Además de este tema, está disponible la siguiente documentación de virtualización de función de la red.  
> - [Información general de Datacenter Firewall](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Puerta de enlace RAS para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Software equilibrio de carga (SLB) para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
En el software de hoy en día centros de datos definidos, las funciones de red que se realizan mediante dispositivos de hardware (por ejemplo, equilibradores de carga, firewalls, enrutadores, conmutadores etc.) se están virtualizando como aplicaciones virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red. Dispositivos virtuales rápidamente se emergentes y creación de un mercado completamente nuevo. Se seguirán generando interés y obtener momentum en ambas plataformas de virtualización y servicios en la nube.  
  
Microsoft incluye una puerta de enlace independiente como una aplicación virtual a partir de Windows Server 2012 R2. Para obtener más información, consulte [Puerta de enlace de Windows Server](https://technet.microsoft.com/library/dn313101.aspx). Ahora con Windows Server 2016, Microsoft continúa expanda e invertir en el mercado de virtualización de función de red.  
  
## <a name="virtual-appliance-benefits"></a>Ventajas de la aplicación virtual  
Un dispositivo virtual es dinámica y fácil de cambiar porque es una máquina virtual pregenerada y personalizada. Puede ser una o varias máquinas virtuales empaquetadas, actualiza y mantiene como una unidad. Junto con el software definido networking (SDN), obtendrá la agilidad y flexibilidad necesarias en la infraestructura en la nube de hoy. Por ejemplo:  
  
-   SDN presenta la red como un recurso dinámico o agrupado.  
  
-   SDN facilita el aislamiento de inquilinos.  
  
-   SDN maximiza el rendimiento y escalabilidad.  
  
-   Aplicaciones virtuales de permiten la movilidad de expansión y la carga de trabajo de la capacidad de conexión directa.  
  
-   Las aplicaciones virtuales de minimizar la complejidad operacional.  
  
-   Aplicaciones virtuales permiten a los clientes adquirir, implementar y administrar soluciones preintegradas fácilmente.  
  
    -   Los clientes pueden mover fácilmente el dispositivo virtual en cualquier lugar en la nube.  
  
    -   Los clientes pueden escalar las aplicaciones virtuales o abajo dinámicamente según la demanda.  
  
Para obtener más información acerca de Microsoft SDN vea [redes definidas por Software](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>¿Qué funciones de red se estaba virtualizando?  
El mercado para las funciones de red virtualizado está creciendo rápidamente. Las siguientes funciones de red virtualizadas:  
  
-   **Seguridad**  
  
    -   Firewall  
  
    -   Antivirus  
  
    -   DDoS (denegación de servicio distribuido)  
  
    -   IPS/IDS (sistema de detección de intrusiones o sistema de prevención de intrusiones)  
  
-   **Optimizadores de WAN/aplicación**  
  
-   **Edge**  
  
    -   Puerta de enlace de sitio a sitio  
  
    -   Puertas de enlace L3  
  
    -   Enrutadores  
  
    -   Modificadores  
  
    -   NAT  
  
    -   Equilibradores de carga (no necesariamente en el borde)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>¿Por qué Microsoft es una plataforma excelente para aplicaciones virtuales  
![Pila de red virtual](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La plataforma de Microsoft se ha diseñado para que sea una plataforma excelente para compilar e implementar aplicaciones virtuales. Aquí es la razón:  
  
-   Microsoft proporciona funciones clave virtualizada de red con Windows Server 2016.  
  
-   Puede implementar una aplicación virtual del proveedor de su elección.  
  
-   Puede implementar, configurar y administrar sus aplicaciones virtuales con la controladora de red de Microsoft que se incluye con Windows Server 2016. Para obtener más información acerca de la controladora de red, consulte [controladora de red](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V puede hospedar los sistemas operativos invitados principales que necesita.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualización de función de red en Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funciones de aplicaciones virtuales proporcionadas por Microsoft  
Se proporcionan las siguientes aplicaciones virtuales con Windows Server 2016:  
  
**Equilibrador de carga de software**  
  
Un equilibrador de carga de nivel 4 trabaja a escala de centro de datos. Se trata de una versión similar de equilibrador de carga de Azure que se haya implementado a escala en el entorno de Azure. Para obtener más información sobre el equilibrador de carga de Software de Microsoft, consulte [equilibrio de carga de Software (SLB) para SDN](https://technet.microsoft.com/library/mt632286.aspx). Para obtener más información acerca de servicios de equilibrio de carga de Microsoft Azure, consulte [servicios de equilibrio de carga de Microsoft Azure](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Puerta de enlace**. Puerta de enlace RAS proporciona todas las combinaciones de las siguientes funciones de puerta de enlace.  
  
-   **Puerta de enlace de sitio a sitio**  
  
    Puerta de enlace RAS proporciona un protocolo de puerta de enlace de borde (BGP)-puerta de enlace para varios inquilinos, puede que permite a los inquilinos acceder y administrar sus recursos a través de conexiones de VPN de sitio a sitio desde sitios remotos y permite el flujo de tráfico de red entre recursos virtuales en las redes físicas en la nube y de inquilinos. Para obtener más información acerca de la puerta de enlace de RAS, consulte [alta disponibilidad de puerta de enlace de RAS](https://technet.microsoft.com/library/mt631692.aspx) y [puerta de enlace RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Puerta de enlace de reenvío**  
  
    Puerta de enlace RAS enruta el tráfico entre redes virtuales y la red física de proveedor de hospedaje. Por ejemplo, si los inquilinos creación uno o más redes virtuales y necesitan tener acceso a los recursos compartidos en la red física en el proveedor de hospedaje, la puerta de enlace de reenvío puede enrutar el tráfico entre la red virtual y la red física para proporcionar a los usuarios que trabajan en la red virtual con los servicios que necesitan. Para obtener más información, consulte [alta disponibilidad de puerta de enlace de RAS](https://technet.microsoft.com/library/mt631692.aspx) y [puerta de enlace RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Puertas de enlace de túnel GRE**  
  
    Los túneles basados en GRE habilitan la conectividad entre redes virtuales de inquilinos y redes externas. Puesto que el protocolo GRE es ligero y compatibilidad con GRE se encuentra disponible en la mayoría de los dispositivos de red, se convierte en una opción ideal para el túnel donde no se requiere el cifrado de datos. La compatibilidad con GRE de los túneles de sitio a sitio (S2S) soluciona el problema de reenvío entre redes virtuales de inquilinos y redes externas de inquilinos mediante una puerta de enlace de varios inquilinos. Para obtener más información acerca de los túneles GRE, consulte [túneles GRE en Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Plano de control de enrutamiento con BGP**  
  
Control del enrutamiento de virtualización de red de Hyper-V (HNV) es la entidad lógica y centralizada en el plano de control, que lleva a cabo todas las rutas de dirección de cliente plano y aprende dinámicamente y, a continuación, actualiza los enrutadores de puerta de enlace RAS distribuidos en la red virtual. Para obtener más información, consulte [alta disponibilidad de puerta de enlace de RAS](https://technet.microsoft.com/library/mt631692.aspx) y [puerta de enlace RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Firewall distribuido de varios inquilinos**  
  
El servidor de seguridad protege la capa de red de redes virtuales. Las directivas se aplican en el puerto de SDN vSwitch de cada máquina virtual del inquilino. Protege todos los flujos de tráfico: Asia occidental y norte sur. Las directivas se insertan a través del portal del inquilino y la controladora de red distribuye a todos los hosts aplicables. Para obtener más información sobre el firewall distribuido de varios inquilinos, consulte [información general de Datacenter Firewall](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


