---
title: Software definido redes (SDN)
description: Puedes usar este tema para obtener información sobre las tecnologías de redes de definido de Software (SDN) que se proporcionan en Microsoft Azure, Windows Server y System Center.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33351ccfa1466f667ef9351768c89b373734075d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="software-defined-networking-sdn"></a>Software definido redes (SDN)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre las tecnologías de redes de definido de Software (SDN) que se proporcionan en Windows Server Datacenter edition, System Center 2016 y Microsoft Azure.  
  
> [!NOTE]
> Además de este tema, el siguiente contenido SDN está disponible.
> 
> - [Novedades en SDN para Windows Server](sdn-whats-new.md)
> - [Introducción a SDN en Windows Server Datacenter](sdn-intro.md)
> - [Tecnologías SDN](technologies/Software-Defined-Networking-Technologies.md)  
> - [Planear SDN](plan/Plan-Software-Defined-Networking.md) 
> - [Implementar SDN](deploy/Deploy-Software-Defined-Networking.md)  
> - [Administrar SDN](manage/manage-sdn.md)
> - [Seguridad para SDN](security/sdn-security-top.md)
> - [Solucionar problemas de SDN](troubleshoot/Troubleshoot-Software-Defined-Networking.md)
> - [Tecnologías de System Center para SDN](Sc-Tech-for-Sdn.md)
> - [Microsoft Azure y SDN](Azure_and_Sdn.md)
> - [Ponte en contacto con el centro de datos y equipo de redes de nube](contact-sdn-team.md)
  
## <a name="bkmk_sdn"></a>Introducción a las redes de definido de software

Software definido redes \(SDN\) proporciona un método para configurar y administrar dispositivos de red físicas y virtuales, como los enrutadores, los conmutadores y puertas de enlace en el centro de datos de forma centralizada. Elementos de la red virtual como conmutador Virtual de Hyper-V, Hyper-V la virtualización de red y puerta de enlace de RAS están diseñados para ser elementos integral de la infraestructura SDN.

>[!NOTE]
>Para los hosts de Hyper-V y \(VMs\) máquinas virtuales que se ejecutan en los servidores de infraestructura SDN, como nodos de controlador de red y equilibrio de carga de Software, debes instalar Windows Server 2016 Datacenter edition. Para el host de Hyper-V que contengan solo carga de trabajo de inquilino máquinas virtuales que estén conectados a SDN\ controladas de redes, se puede ejecutar Windows Server 2016 Standard edition.

Aunque todavía puede usar los conmutadores físicos existentes, enrutadores y otros dispositivos de hardware, puedes lograr una mayor integración entre la red virtual y la red física si estos dispositivos están diseñados para la compatibilidad con redes de software definido.  
  
SDN es posible porque los planos de red: los planos de administración, control y datos - ya no están enlazados a los dispositivos de red a sí mismos, pero otras entidades, como software de administración de centros de datos como System Center 2016 abstraen para su uso.  
  
SDN te permite administrar dinámicamente la red de centros de datos para proporcionar una manera automatizada, centralizada para cumplir los requisitos de las aplicaciones y las cargas de trabajo. Redes de software definido proporciona las siguientes funcionalidades.  
  
-   La capacidad para abstraer sus aplicaciones y las cargas de trabajo de la red subyacente física, lo que se consigue mediante la virtualización de la red. Al igual que con la virtualización de servidor usando Hyper-V, las abstracciones sean coherentes y trabajo con las aplicaciones y las cargas de trabajo de manera sin interrupciones. Por ejemplo, redes software definido proporcionan abstracciones virtuales para los elementos de la red física, como direcciones IP, cambia y equilibrados de carga.  
  
-   Las directivas de control que rigen redes físicas y virtuales, incluido el tráfico y la posibilidad de definir de forma centralizada fluyen entre estos dos tipos de red.  
  
-   La capacidad para implementar directivas de red de una manera coherente a escala, incluso al implementar nuevas cargas de trabajo o mover cargas de trabajo a través de redes virtuales o físicas.  
  
## <a name="bkmk_ws"></a>Tecnologías de servidor de Windows para Software definen redes  
Windows Server incluye las siguientes tecnologías de redes de software definido.  

### <a name="bkmk_nc"></a>Controlador de red

Novedad en Windows Server 2016, controlador de red proporciona un punto centralizado, programable de automatización para administrar, configurar, supervisar y solucionar problemas de ambos virtual y la infraestructura de red física en el centro de datos. Usar el controlador de red, puede automatizar la configuración de la infraestructura de red en lugar de realizar la configuración manual de dispositivos de red y servicios.  
  
Controlador de red es un rol de servidor altamente disponible y escalable y proporciona una aplicación de programación interfaz (API) - la API Southbound - que permite que el controlador de red para comunicarse con la red y una segunda API - la API Northbound - que le permite comunicarse con el controlador de red.  
  
Con Windows PowerShell, la API de transferencia de estado representacional (REST) o una aplicación de administración, puedes usar el controlador de red para administrar la infraestructura de red físicas y virtuales siguientes.  
  
-   Máquinas virtuales de Hyper-V y conmutadores virtuales  
  
-   Modificadores de física de red  
  
-   Enrutadores de red física  
  
-   Software de Firewall  
  
-   Puertas de enlace de VPN, incluidas las puertas de enlace Multitenant de servicio de acceso remoto (RAS)  
  
-   Equilibradores de carga  
  
Para obtener más información, consulta [controlador de red](../sdn/technologies/network-controller/Network-Controller.md).  
  
### <a name="bkmk_hv"></a>Virtualización de la red de Hyper-V

Virtualización de Hyper-V red te ayuda a abstraer sus aplicaciones y las cargas de trabajo de la red mediante el uso de redes virtuales. Redes virtuales proporcionan el aislamiento es necesario multiempresa mientras se ejecuta en una estructura física de red compartido, lo que aumento de la utilización de recursos. Para garantizar que se pueden transferir sus inversiones, puedes configurar las redes virtuales en el equipo de red existente. Además, las redes virtuales son compatibles con redes de área Local virtual (VLAN).  
  
Para obtener más información, consulta [virtualización de Hyper-V red](../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  
  
### <a name="bkmk_switch"></a>Conmutador Virtual de Hyper-V

El conmutador Virtual de Hyper-V es un basado en software nivel 2 Ethernet conmutador de red que está disponible en el Administrador de Hyper-V después de haber instalado el rol de Hyper-V server. El conmutador incluye funcionalidades mediante programación y no administrados extensibles para conectar máquinas virtuales con redes virtuales y la física de red. Además, conmutador Virtual de Hyper-V proporciona la aplicación de directivas de seguridad, aislamiento y niveles de servicio.  
  
En el conmutador Virtual de Hyper-V en Windows Server 2016, también puedes implementar conmutador incrustado agrupación (conjunto) y memoria de acceso directo remoto (RDMA). Para obtener más información, consulta la sección [memoria de acceso directo remoto (RDMA) y cambiar incrustado agrupación (conjunto)](#bkmk_rdma) en este tema.  
  
Para obtener más información sobre el conmutador Virtual de Hyper-V, consulta [conmutador Virtual de Hyper-V](../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="bkmk_idns"></a>Interno de servicio DNS & #40; IDN & #41;

\(VMs\) hospedado máquinas virtuales y las aplicaciones requieren DNS para la comunicación entrante en sus propias redes con recursos externos en Internet. Con IDN, puedes proporcionar inquilinos con servicios de resolución de nombres DNS de su espacio de nombres aislado, local y de recursos de Internet.

Para obtener más información, consulta [servicio DNS interno & #40; IDN & #41; para SDN](technologies/Idns-for-Sdn.md).
  
### <a name="bkmk_nfv"></a>Virtualización de la función de red

En el software de hoy en día cada vez más se se virtualizan los centros de datos definidos, funciones de red que se realiza en dispositivos de hardware (como equilibradores de carga, firewalls, enrutadores, conmutadores y así sucesivamente) como dispositivos virtuales. Este "virtualización en función de red" es un progreso natural de la virtualización de servidor y la virtualización de la red. Dispositivos virtuales rápidamente son emergentes y creación de un mercado totalmente nuevo. Se seguirán generar interés y obtener impulso en ambas plataformas de virtualización y servicios en la nube.  
  
Están disponibles las siguientes tecnologías de virtualización de la función de red.  
  
-   **Equilibrado de carga de software (SLB) y de red (NAT) de la traducción de direcciones**. El norte sur y Oriental oeste de nivel 4 equilibrado de carga y NAT mejora el rendimiento al admitir directa servidor devolver, con la que puede omitir el tráfico de red devuelto el equilibrio de carga multiplexor. Para obtener más información, consulta [equilibrio de carga de Software (SLB) para SDN](technologies/network-function-virtualization/software-load-balancing-for-sdn.md).  
  
-   **Datacenter Firewall**. Este firewall distribuida ofrece listas de control de acceso granular (ACL), lo que permite aplicar directivas de firewall en el nivel de la interfaz de máquina virtual o en el nivel de subred.  
  
    Para obtener más información, consulta [Datacenter Firewall Introducción](../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
-   **Puerta de enlace RAS**. Puedes usar las puertas de enlace para el puente tráfico entre redes virtuales y redes no virtualizados; específicamente, puedes implementar puertas de enlace de sitio a sitio VPN, puertas de enlace de reenvío y puertas de enlace de encapsulación de enrutamiento genérico (GRE). Además, M + N redundancia puertas de enlace es compatible. Para obtener más información, consulta [RAS puerta de enlace SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).
  
Para obtener más información, consulta [virtualización de la función de red](technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_rdma"></a>Remoto acceso directo a memoria (RDMA) y el modificador incrustan colaboración (conjunto)  
En Windows Server 2016, puedes habilitar RDMA en adaptadores de red que están enlazados a un conmutador Virtual de Hyper-V con o sin modificador incrustado agrupación (configurado). Esto te permite usar menos adaptadores de red al que quieras usar RDMA y conjunto al mismo tiempo.  
  
CONJUNTO es una solución alternativa equipo NIC que puedes usar en entornos que incluyen Hyper-V y la pila de Software definido de redes (SDN) en Windows Server 2016. CONJUNTO integra algunas de las funciones de equipo NIC en el conmutador de Hyper-V Virtual.  
  
CONJUNTO te permite agrupar entre uno y ocho físicos adaptadores de red Ethernet en uno o varios adaptadores de red virtual basado en software. Estos adaptadores de red virtual proporcionan rápido rendimiento y la tolerancia a errores en caso de error de adaptador de red.  
Adaptadores de red de conjunto de miembros deben instalarse en el mismo host Hyper-V físico debe colocarse en un equipo.  
  
Además, puedes usar comandos de Windows PowerShell para habilitar el puente de centro de datos (DCB), crea un conmutador Virtual de Hyper-V con una NIC virtual de RDMA (vNIC) y crea un conmutador Virtual de Hyper-V con conjunto y RDMA vNICs.  
  
Para obtener más información, consulta [memoria de acceso directo remoto (RDMA) y cambiar incrustado agrupación (conjunto)](https://technet.microsoft.com/library/mt403349.aspx).  
  
### <a name="bkmk_rras"></a>Puerta de enlace RAS para SDN  
Puerta de enlace de RAS es un protocolo de puerta de enlace de borde (BGP) basado en software y para varios inquilinos, compatible con enrutador en Windows Server 2016 está diseñada para proveedores de servicios de nube (CSP) y las empresas que hospedar varias redes virtuales inquilino usando la virtualización de Hyper-V red.  
  
Puerta de enlace de RAS proporciona grupos de puerta de enlace, redundancia M + N, varios tipos de conexiones de VPN de sitio a sitio y BGP espejo de ruta para proporcionar opciones de diseño flexible para la infraestructura de puerta de enlace.  
  
Para obtener más información, consulta [RAS puerta de enlace SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
  
### <a name="bkmk_slb"></a>(SLB) de equilibrio de carga de software  
Proveedores de servicios de nube (CSP) y las empresas que implementan Software definido de redes (SDN) en Windows Server 2016 usar Equilibrio de carga de Software (SLB) para distribuir uniformemente inquilino y tráfico de red de cliente de inquilino entre los recursos de red virtual. La SLB de servidor de Windows permite varios servidores hospedar la misma carga de trabajo que escalabilidad y alta disponibilidad.  
  
Para obtener más información, consulta [equilibrio de carga de Software & #40; SLB & #41; para SDN](../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md).

### <a name="windows-server-containers"></a>Contenedores de servidor de Windows

Windows Server contenedores son un método de virtualización de sistema operativo ligero usado para separar los servicios o aplicaciones de otros servicios que se ejecutan en el mismo host de contenedor. Para habilitar esto, cada contenedor tiene su propia vista del sistema operativo, procesos, sistema de archivos, registro y direcciones IP. Con Windows Server 2016, ahora puede conectar a redes virtuales contenedores de Windows Server. Para obtener más información, consulta [contenedores de Windows Server](technologies/containers/Container-networking-overview.md).

### <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>Ponte en contacto con el equipo de producto de la nube de red y Datacenter

Si estás interesado en debatir temas como las tecnologías SDN con Microsoft o de otros clientes SDN, hay una variedad de métodos para hacer contacto.

Para obtener más información, consulta [póngase en contacto con el equipo de redes de nube y Datacenter](contact-sdn-team.md).
