---
title: Virtualización de la función de red
description: Puedes usar este tema para obtener información sobre la virtualización de función de red, que permite implementar los dispositivos de red virtuales como Datacenter Firewall, multiempresa RAS puerta de enlace y equilibrio de carga de Software (SLB) en Windows Server 2016.
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
ms.openlocfilehash: 7caa9eef42cb7ab95a13d64c1dcd3639b1132eb3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-function-virtualization"></a>Virtualización de la función de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre la virtualización de función de red, que permite implementar los dispositivos de red virtuales como Datacenter Firewall, multitenant RAS puerta de enlace y equilibrio de carga de Software \(SLB\) \(MUX\) multiplexor.
  
>[!NOTE]  
>Además de este tema, la siguiente documentación de virtualización de la función de red está disponible.  
> - [Introducción a Datacenter Firewall](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Puerta de enlace RAS para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Software equilibrio de carga (SLB) para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
En el software de hoy en día cada vez más se se virtualizan los centros de datos definidos, funciones de red que se realiza en dispositivos de hardware (como equilibradores de carga, firewalls, enrutadores, conmutadores y así sucesivamente) como dispositivos virtuales. Este "virtualización en función de red" es un progreso natural de la virtualización de servidor y la virtualización de la red. Dispositivos virtuales rápidamente son emergentes y creación de un mercado totalmente nuevo. Se seguirán generar interés y obtener impulso en ambas plataformas de virtualización y servicios en la nube.  
  
Microsoft incluyó una puerta de enlace independiente, como un dispositivo virtual a partir de Windows Server 2012 R2. Para obtener más información, consulta [puerta de enlace de servidor de Windows](https://technet.microsoft.com/library/dn313101.aspx). Ahora con Windows Server 2016 Microsoft sigue expandir e invertir en el mercado de virtualización de función de red.  
  
## <a name="virtual-appliance-benefits"></a>Ventajas de dispositivo virtual  
Un dispositivo virtual es dinámico y fácil cambiar porque es una máquina virtual predefinida y personalizada. Puede ser uno o varios equipos virtuales empaquetado, actualizan y mantienen como una unidad. Define junto con el software de redes (SDN), obtendrás la agilidad y la flexibilidad necesaria en la infraestructura de hoy en la nube. Por ejemplo:  
  
-   SDN presenta la red como un recurso agrupado y dinámico.  
  
-   SDN facilita el aislamiento de inquilino.  
  
-   SDN maximiza la escala y el rendimiento.  
  
-   Los dispositivos virtuales permiten movilidad de expansión y carga de trabajo de capacidad sin problemas.  
  
-   Dispositivos virtuales minimizar la complejidad operativa.  
  
-   Dispositivos virtuales permiten a los clientes adquirir, implementar y administrar las soluciones integradas previamente fácilmente.  
  
    -   Los clientes pueden mover con facilidad el dispositivo virtual en cualquier lugar en la nube.  
  
    -   Los clientes pueden aumentar el tamaño dispositivos virtuales o hacia abajo dinámicamente de acuerdo a petición.  
  
Para obtener más información sobre Microsoft SDN consulta [definido redes Software](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>¿Virtualizada de las funciones de red?  
El mercado de las funciones de red virtualizada está creciendo rápidamente. Virtualizada de las siguientes funciones de red:  
  
-   **Seguridad**  
  
    -   Firewall de  
  
    -   Antivirus  
  
    -   DDoS (distribuida denegación de servicio)  
  
    -   IPS/IDS (sistema de detección de intrusiones/sistema de prevención de intrusiones)  
  
-   **Optimizadores de la aplicación o WAN**  
  
-   **Borde**  
  
    -   Puerta de enlace de sitio a sitio  
  
    -   Puertas de enlace L3  
  
    -   Enrutadores  
  
    -   Modificadores  
  
    -   NAT  
  
    -   Cargar equilibradores (no necesariamente en el borde)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>¿Por qué Microsoft es una excelente plataforma para dispositivos virtuales  
![Pila de red virtual](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La plataforma de Microsoft se ha diseñado para que sea una excelente plataforma para compilar e implementar los dispositivos virtuales. Estas son las razones:  
  
-   Microsoft proporciona funciones de red virtualizada clave con Windows Server 2016.  
  
-   Puedes implementar un dispositivo virtual del proveedor de tu elección.  
  
-   Puedes implementar, configurar y administrar los dispositivos virtuales con el controlador de red de Microsoft que viene con Windows Server 2016. Para obtener más información sobre el controlador de red, consulta [controlador de red](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V puede hospedar los sistemas operativos de invitado superior que necesitas.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualización de la función de red en Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funciones de dispositivos virtuales proporcionadas por Microsoft  
Los siguientes dispositivos virtuales se proporcionan con Windows Server 2016:  
  
**Equilibrado de carga de software**  
  
Un equilibrado de carga de nivel 4 operativo a una escala del centro de datos. Esta es una versión similar de equilibrado de carga de Azure que se haya implementado a escala en el entorno de Azure. Para obtener más información sobre el equilibrado de carga de Software de Microsoft, consulta [equilibrio de carga de Software (SLB) para SDN](https://technet.microsoft.com/library/mt632286.aspx). Para obtener más información acerca de los servicios de equilibrio de carga de Microsoft Azure, consulta [servicios de equilibrio de carga de Microsoft Azure](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Puerta de enlace**. Puerta de enlace de RAS proporciona todas las combinaciones de las siguientes funciones de puerta de enlace.  
  
-   **Puerta de enlace de sitio a sitio**  
  
    Puerta de enlace de RAS proporciona un protocolo de puerta de enlace de borde (BGP)-capaz y para varios inquilinos puerta de enlace que permite los inquilinos tener acceso y administrar sus recursos a través de conexiones de VPN de sitio a sitio desde sitios remotos, y que permita el flujo de tráfico de red entre los recursos virtuales en las redes en la nube e inquilino físicas. Para obtener más información acerca de la puerta de enlace de RAS, consulta [RAS puerta de enlace de alta disponibilidad](https://technet.microsoft.com/library/mt631692.aspx) y [puerta de enlace de RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Puerta de enlace de desvío de llamadas**  
  
    Puerta de enlace de RAS enruta el tráfico entre redes virtuales y el hospedaje red física de proveedor. Por ejemplo, si inquilinos crear uno o más redes virtuales y necesitan tener acceso a recursos compartidos de la red en el proveedor de hospedaje física, la puerta de enlace de desvío de llamadas puede enrutar el tráfico entre la red virtual y la red física para proporcionar a los usuarios que trabajan en la red virtual con los servicios que necesitan. Para obtener más información, consulta [RAS puerta de enlace de alta disponibilidad](https://technet.microsoft.com/library/mt631692.aspx) y [puerta de enlace de RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Puertas de enlace de túnel GRE**  
  
    GRE basa túneles Habilitar conectividad entre redes virtuales del inquilino y redes externas. Dado que el protocolo GRE es ligero y soporte técnico para GRE está disponible en la mayoría de los dispositivos de red, se convierte en idóneo túnel donde no se requiere el cifrado de datos. Soporte técnico GRE en los túneles (S2S) de un sitio a otro solucionó el problema de reenvío entre redes virtuales de inquilino y redes externas inquilino con una puerta de enlace de inquilino múltiples. Para obtener más información acerca de los túneles GRE, consulta [GRE túnel en Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Enrutamiento plano control con BGP**  
  
Control de enrutamiento de virtualización de red de Hyper-V (HNV) es la entidad lógica, centralizada en el plano de control, que lleva a cabo todas las rutas del plano de dirección de cliente y aprende dinámicamente y, a continuación, actualiza los enrutadores puerta de enlace de RAS distribuidos en la red virtual. Para obtener más información, consulta [RAS puerta de enlace de alta disponibilidad](https://technet.microsoft.com/library/mt631692.aspx) y [puerta de enlace de RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Firewall de varios inquilino distribuido**  
  
El firewall protege el nivel de red de redes virtuales. Las directivas se aplican en el puerto SDN vSwitch de cada inquilino de máquina virtual. Protege todos los flujos de tráfico: oriental oeste y norte sur. Las directivas se insertan a través del portal de inquilino y distribuye el controlador de red a todos los hosts aplicables. Para obtener más información sobre el firewall de varios inquilino distribuido, consulta [Datacenter Firewall Introducción](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


