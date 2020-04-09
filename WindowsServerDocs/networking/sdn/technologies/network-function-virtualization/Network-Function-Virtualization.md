---
title: Virtualización de función de red
description: Puede usar este tema para obtener información sobre la virtualización de función de red, que le permite implementar aplicaciones de red virtual como firewall de centro de seguridad de red, puerta de enlace RAS multiempresa y equilibrio de carga de software (SLB) en Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: aed1591756e7b491bd4c9ab325694dfb3e6fddb1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859628"
---
# <a name="network-function-virtualization"></a>Virtualización de función de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre la virtualización de función de red, que le permite implementar aplicaciones de red virtual como firewall de centro de seguridad de red, puerta de enlace RAS multiinquilino y equilibrio de carga de software \(multiplexador de\) de SLB \(MUX\).
  
>[!NOTE]  
>Además de este tema, está disponible la siguiente documentación sobre virtualización de funciones de red.  
> - [Información general de Datacenter Firewall](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Puerta de enlace RAS para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Equilibrio de carga de software (SLB) para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
En los centros de recursos definidos por software de hoy en día, las funciones de red que realizan los dispositivos de hardware (como equilibradores de carga, firewalls, enrutadores, conmutadores, etc.) se virtualizan cada vez más como aplicaciones virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red. Las aplicaciones virtuales están surgiendo rápidamente y crean un nuevo mercado. Continúan generando interés y ganar impulsos en las plataformas de virtualización y en los servicios en la nube.  
  
Microsoft incluyó una puerta de enlace independiente como un dispositivo virtual a partir de Windows Server 2012 R2. Para obtener más información, vea [Puerta de enlace de Windows Server](https://technet.microsoft.com/library/dn313101.aspx). Ahora con Windows Server 2016, Microsoft continúa expandiendo e invirtiendo en el mercado de virtualización de función de red.  
  
## <a name="virtual-appliance-benefits"></a>Ventajas del dispositivo virtual  
Un dispositivo virtual es dinámico y fácil de cambiar porque es una máquina virtual pregenerada y personalizada. Puede ser una o varias máquinas virtuales empaquetadas, actualizadas y mantenidas como una unidad. Junto con las redes definidas por software (SDN), obtiene la agilidad y la flexibilidad que se necesitan en la infraestructura basada en la nube de hoy en día. Por ejemplo:  
  
-   SDN presenta la red como un recurso agrupado y dinámico.  
  
-   SDN facilita el aislamiento de inquilinos.  
  
-   SDN maximiza la escala y el rendimiento.  
  
-   Los dispositivos virtuales permiten la expansión de la capacidad y la movilidad de la carga de trabajo.  
  
-   Las aplicaciones virtuales minimizan la complejidad operativa.  
  
-   Los dispositivos virtuales permiten a los clientes adquirir, implementar y administrar soluciones previamente integradas.  
  
    -   Los clientes pueden trasladar fácilmente el dispositivo virtual en cualquier parte de la nube.  
  
    -   Los clientes pueden escalar o reducir verticalmente dispositivos virtuales de forma dinámica en función de la demanda.  
  
Para obtener más información sobre Microsoft SDN, vea [redes definidas por software](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>¿Qué funciones de red se están virtualizando?  
El Marketplace de las funciones de red virtualizada está creciendo rápidamente. Se virtualizan las siguientes funciones de red:  
  
-   **Seguridad**  
  
    -   Firewall  
  
    -   Antivirus  
  
    -   DDoS (denegación de servicio distribuida)  
  
    -   IP/IDS (sistema de prevención de intrusiones/sistema de detección de intrusiones)  
  
-   **Optimizadores de aplicaciones/WAN**  
  
-   **Edge**  
  
    -   Puerta de enlace de sitio a sitio  
  
    -   Puertas de enlace L3  
  
    -   Enrutadores  
  
    -   Conmutadores  
  
    -   NAT  
  
    -   Equilibradores de carga (no necesariamente en el perímetro)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Por qué Microsoft es una plataforma excelente para aplicaciones virtuales  
![Pila de red virtual](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La plataforma de Microsoft se ha diseñado para ser una plataforma excelente para compilar e implementar aplicaciones virtuales. Este es el motivo:  
  
-   Microsoft proporciona funciones clave de red virtualizada con Windows Server 2016.  
  
-   Puede implementar un dispositivo virtual desde el proveedor de su elección.  
  
-   Puede implementar, configurar y administrar sus aplicaciones virtuales con la controladora de red de Microsoft que viene con Windows Server 2016. Para obtener más información acerca de la controladora de red, consulte [controladora de red](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V puede hospedar los sistemas operativos invitados principales que necesita.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualización de función de red en Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funciones de los dispositivos virtuales proporcionadas por Microsoft  
En Windows Server 2016 se proporcionan las siguientes aplicaciones virtuales:  
  
**Equilibrador de carga de software**  
  
Un equilibrador de carga de nivel 4 que funciona en la escala del centro de recursos. Se trata de una versión similar del equilibrador de carga de Azure que se ha implementado a escala en el entorno de Azure. Para obtener más información sobre el Load Balancer de software de Microsoft, consulte [equilibrio de carga de software (SLB) para Sdn](https://technet.microsoft.com/library/mt632286.aspx). Para obtener más información acerca de los servicios de equilibrio de carga de Microsoft Azure, consulte [Microsoft Azure servicios de equilibrio de carga](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Puerta de enlace**. La puerta de enlace RAS proporciona todas las combinaciones de las siguientes funciones de puerta de enlace.  
  
-   **Puerta de enlace de sitio a sitio**  
  
    La puerta de enlace RAS proporciona una puerta de enlace multiinquilino compatible con Protocolo de puerta de enlace de borde (BGP) que permite a los inquilinos tener acceso a sus recursos y administrarlos a través de conexiones VPN de sitio a sitio desde sitios remotos y que permite el flujo de tráfico de red entre recursos virtuales. en las redes físicas de nube y de inquilino. Para obtener más información sobre la puerta de enlace RAS, [consulte puerta de enlace ras y](https://technet.microsoft.com/library/mt626650.aspx) [alta disponibilidad](https://technet.microsoft.com/library/mt631692.aspx) .  
  
-   **Reenvío de puerta de enlace**  
  
    La puerta de enlace RAS enruta el tráfico entre las redes virtuales y la red física del proveedor de hospedaje. Por ejemplo, si los inquilinos crean una o más redes virtuales y necesitan acceso a recursos compartidos de la red física en el proveedor de hospedaje, la puerta de enlace de reenvío puede enrutar el tráfico entre la red virtual y la red física para proporcionar a los usuarios que trabajan en la red virtual con los servicios que necesitan. Para obtener más información, vea puerta de enlace [ras alta disponibilidad](https://technet.microsoft.com/library/mt631692.aspx) y [puerta de enlace ras](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Puertas de enlace de túnel GRE**  
  
    Los túneles basados en GRE habilitan la conectividad entre redes virtuales de inquilinos y redes externas. Dado que el protocolo GRE es ligero y la compatibilidad con GRE está disponible en la mayoría de los dispositivos de red, se convierte en una opción ideal para el túnel donde no se requiere el cifrado de datos. La compatibilidad con GRE de los túneles de sitio a sitio (S2S) soluciona el problema de reenvío entre redes virtuales de inquilinos y redes externas de inquilinos mediante una puerta de enlace de varios inquilinos. Para obtener más información acerca de los túneles GRE, consulte [tunelización de GRE en Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Plano de control de enrutamiento con BGP**  
  
El control de enrutamiento de virtualización de red de Hyper-V (HNV) es la entidad lógica y centralizada en el plano de control, que incluye todas las rutas de plano de dirección del cliente y aprende dinámicamente y, a continuación, actualiza los enrutadores de puerta de enlace RAS distribuidos en la red virtual. Para obtener más información, vea puerta de enlace [ras alta disponibilidad](https://technet.microsoft.com/library/mt631692.aspx) y [puerta de enlace ras](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Firewall multiinquilino distribuido**  
  
El Firewall protege el nivel de red de las redes virtuales. Las directivas se aplican en el puerto SDN-vSwitch de cada máquina virtual de inquilino. Protege todos los flujos de tráfico: este-oeste y norte-sur. Las directivas se insertan a través del portal del inquilino y la controladora de red las distribuye a todos los hosts aplicables. Para obtener más información sobre el Firewall multiinquilino distribuido, vea [información general](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)sobre el Firewall de centros de datos.  
  


