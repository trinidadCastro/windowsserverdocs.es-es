---
title: Funciones de red
description: Este tema proporciona una visión general de las tecnologías de redes definidas por software y plataforma de redes que están disponibles en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 7caa99f1b6b9e25e5a6f2c4333b033fb3088195d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862596"
---
# <a name="networking"></a>Funciones de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> Las redes son una parte fundamental del centro de datos de definida de Software \(SDDC\) platform y Windows Server 2016 proporciona nuevas y mejoradas redes definidas por Software \(SDN\) tecnologías que le ayudarán a mover a una solución SDDC producida por completo para su organización.

Al administrar las redes como un recurso definido por software, puede describir una vez los requisitos de infraestructura de la aplicación y, después, elegir dónde se ejecuta la aplicación: localmente o en la nube. 

Esta coherencia significa que las aplicaciones son ahora más fáciles de escalar y que puedes ejecutar sin problemas las aplicaciones, en cualquier lugar, con la misma confianza en lo que respecta a la seguridad, el rendimiento, la calidad del servicio y la disponibilidad.

>[!Note]
> Para descargar Windows Server, consulta [Evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

Windows Server 2016 agrega las nuevas tecnologías de redes que se indican a continuación:

- Redes definidas por software: La controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma. Controladora de red permite usar virtualización de red de función para implementar fácilmente máquinas virtuales \(máquinas virtuales\) para equilibrio de carga de Software \(SLB\) para optimizar el tráfico de red de carga de los inquilinos y RAS Puertas de enlace para proporcionar a los inquilinos con las opciones de conectividad que necesitan entre Internet, en el entorno local y en la nube los recursos. También puede usar la controladora de red para administrar Datacenter Firewall en hosts de Hyper-V y máquinas virtuales.

- Plataforma de red: Con las nuevas características para tecnologías de plataforma de red existentes, puede usar Directiva de DNS para personalizar las respuestas del servidor DNS a las consultas, use una NIC convergente que identificadores de combinarán el acceso a memoria directa remota \(RDMA\) y tráfico de Ethernet , usar Switch Embedded Teaming \(establecer\) para crear conmutadores virtuales de Hyper-V conectados a NIC RDMA y usar Administración de direcciones IP \(IPAM\) para administrar zonas DNS y servidores, así como las direcciones IP y DHCP.

Para obtener más información, consulta [Escenarios de redes admitidos en Windows Server](windows-server-supported-networking-scenarios.md).

Las secciones siguientes proporcionan información sobre tecnologías SDN y tecnologías de plataformas de red.

## <a name="software-defined-networking-technologies"></a>Tecnologías de redes definidas por software

### <a name="software-defined-networking-40sdn41sdnsoftware-defined-networkingmd"></a>[Redes definidas por software &#40;SDN&#41;](sdn/software-defined-networking.md)

Puede usar este tema para obtener información sobre las tecnologías de SDN que se proporcionan en Windows Server, System Center y Microsoft Azure.

>[!NOTE]
>Para hosts de Hyper-V y máquinas virtuales \(máquinas virtuales\) que ejecutan servidores de infraestructura SDN, como nodos de controladora de red y el equilibrio de carga de Software, debe instalar Windows Server 2016 Datacenter edition. Para Hyper-V hosts que contienen solo las máquinas virtuales de carga de trabajo que están conectadas a SDN de inquilinos\-controla las redes, puede ejecutar Windows Server 2016 Standard edition.

### <a name="deploy-a-software-defined-network-infrastructure-using-scriptssdndeploydeploy-a-software-defined-network-infrastructure-using-scriptsmd"></a>[Implementar una infraestructura de red definida por Software con scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
En esta guía se proporcionan instrucciones sobre cómo implementar una controladora de red con redes virtuales y puertas de enlace en un entorno de laboratorio de pruebas.

### <a name="network-controllersdntechnologiesnetwork-controllernetwork-controllermd"></a>[Controladora de red](sdn/technologies/network-controller/Network-Controller.md)

La controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma.

### <a name="software-load-balancing-40slb41-for-sdnsdntechnologiesnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[Equilibrio de carga de software &#40;SLB&#41; para SDN](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

Los proveedores de servicios de nube \(CSP\) y las empresas que implementan definidas por Software Networking (SDN) en Windows Server 2016 pueden usar Equilibrio de carga de Software \(SLB\) distribuir uniformemente el inquilino y tráfico de red de cliente entre los recursos de red virtual. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.

### <a name="ras-gateway-for-sdnsdntechnologiesnetwork-function-virtualizationras-gateway-for-sdnmd"></a>[Puerta de enlace RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

Puerta de enlace de RAS, que es una basada en software para varios inquilinos, Border Gateway Protocol \(BGP\) capaz enrutador en Windows Server 2016, está diseñado para los proveedores de servicios de nube \(CSP\) y las empresas que hospedan varias redes virtuales de inquilinos mediante la virtualización de red de Hyper-V. 

### <a name="network-function-virtualizationsdntechnologiesnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualización de red de función](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

En los centros de datos definidas por software, las funciones que se realizan mediante dispositivos de hardware de red \(como equilibradores de carga, firewalls, enrutadores, conmutadores y así sucesivamente\) están virtualizando como aplicaciones virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red.

### <a name="datacenter-firewall-overviewsdntechnologiesnetwork-function-virtualizationdatacenter-firewall-overviewmd"></a>[Información general de Datacenter Firewall](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Datacenter Firewall es un firewall multiinquilino con estado, de nivel de red y 5-tupla (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino).

## <a name="bkmk_networking"></a>Tecnologías de redes

En la tabla siguiente se proporcionan vínculos a algunas de las tecnologías de redes de Windows Server 2016.

### <a name="whats-new-in-networkingnetworkingwhat-s-new-in-networkingmd"></a>[Novedades de redes](../networking/What-s-New-in-Networking.md)

Puedes usar las siguientes secciones para descubrir nuevas tecnologías de redes y nuevas funciones para tecnologías existentes en Windows Server 2016.

### <a name="branchcachebranchcachebranchcachemd"></a>[BranchCache](branchcache/BranchCache.md)

BranchCache es una red de área extensa \(WAN\) tecnología de optimización del ancho de banda. Para mejorar el ancho de banda de la WAN cuando los usuarios acceden a contenido de servidores remotos, BranchCache obtiene el contenido de los servidores de contenido de la oficina central o de la nube hospedada y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales accedan al contenido de forma local, y no a través de la WAN.

### <a name="core-network-guide-for-windows-server-2016core-network-guidecore-network-guide-windows-servermd"></a>[Guía de red principal para Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Aprende a implementar una red de Windows Server con la guía de red principal, así como a agregar funciones a la implementación de tu red con guías de complemento de red principal.

### <a name="directaccessremoteremote-accessdirectaccessdirectaccessmd"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess permite la conectividad de los usuarios remotos para los recursos de red de la organización. 

La documentación de DirectAccess se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016, en [Acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Para obtener más información, consulta [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### <a name="domain-name-system-40dns41dnsdns-topmd"></a>[Sistema de nombres de dominio &#40;DNS&#41;](dns/dns-top.md)

El sistema de nombres de dominio \(DNS\) es una serie de protocolos estándar del sector que incluyen TCP/IP. De manera conjunta, el cliente DNS y el servidor DNS proporcionan servicios de resolución de nombres de asignación de nombres de equipo a direcciones IP para equipos y usuarios.

### <a name="dynamic-host-configuration-protocol-40dhcp41technologiesdhcpdhcp-topmd"></a>[Protocolo de configuración dinámica de Host &#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

El protocolo de configuración dinámica de host \(DHCP\) es un protocolo cliente/servidor que proporciona automáticamente un host de protocolo de Internet \(IP\) con su dirección IP y otra información de configuración relacionada, como la máscara de subred y la puerta de enlace de predeterminada.

### <a name="hyper-v-network-virtualizationsdntechnologieshyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualización de red de Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

La Virtualización de red de Hyper-V \(HNV\) permite la virtualización de redes de cliente en una infraestructura de red física compartida.

### <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Conmutador Virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

El conmutador virtual de Hyper-V es un conmutador de red Ethernet de nivel 2 basado en software que está disponible en el Administrador de Hyper-V cuando se instala el rol de servidor de Hyper-V. El conmutador incluye mediante programación funcionalidades extensibles y administradas para conectar máquinas virtuales a la red física y a redes virtuales. Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad. 

La documentación del conmutador Virtual de Hyper-V se encuentra ahora en la sección **Virtualización** del índice de Windows Server 2016. Para obtener más información, consulte [Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="ip-address-management-40ipam41technologiesipamipam-topmd"></a>[Administración de direcciones IP &#40;IPAM&#41;](technologies/ipam/ipam-top.md)

La administración de direcciones IP \(IPAM\) es un conjunto de herramientas integrado que permite el planeamiento, la implementación, la administración y la supervisión de un extremo a otro de la infraestructura de direcciones IP, con una experiencia de usuario avanzada. IPAM detecta automáticamente los servidores de infraestructura de direcciones IP y el sistema de nombres de dominio \(DNS\) servidores de la red y permite administrarlos desde una interfaz central.

### <a name="network-load-balancingtechnologiesnetwork-load-balancingmd"></a>[Equilibrio de carga de red](technologies/Network-Load-Balancing.md)

El equilibrio de carga de red \(NLB\) distribuye el tráfico entre varios servidores mediante el protocolo de red TCP/IP. En el caso de implementaciones que no son de SDN, NLB garantiza que las aplicaciones sin estado, como un servidor web que ejecuta Internet Information Services \(IIS\), sean escalables al agregar más servidores a medida que aumenta la carga.

### <a name="high-performance-networkingtechnologieshpnhpn-topmd"></a>[Redes de alto rendimiento](technologies/hpn/hpn-top.md)

Las tecnologías de descarga de la red y optimización en Windows Server 2016 incluyen funciones y tecnologías de solo software (SO), funciones integradas de software y hardware (SH) y funciones y tecnologías de solo Hardware (HO).

La siguiente documentación de tecnología de descarga y optimización también está disponible.

- [Guía de configuración de tarjeta (NIC) de interfaz de red convergente](technologies/conv-nic/cnic-top.md)
- [Centro de datos (DCB) de protocolo de puente](technologies/dcb/dcb-top.md)
- [Virtual escala de recepción (vRSS)](technologies/vrss/vrss-top.md)


### <a name="network-policy-servertechnologiesnpsnps-topmd"></a>[Servidor de directivas de red](technologies/nps/nps-top.md)

Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.

### <a name="network-shell-netshtechnologiesnetshnetshmd"></a>[Shell de red (Netsh)](technologies/netsh/netsh.md)

Puede usar el Shell de red \(netsh\) redes utilidad para administrar tecnologías de red en Windows Server 2016 y Windows 10.

### <a name="network-subsystem-performance-tuningtechnologiesnetwork-subsystemnet-sub-performance-topmd"></a>[Rendimiento del subsistema de red de optimización](technologies/network-subsystem/net-sub-performance-top.md)

En este tema se proporciona información sobre cómo elegir el adaptador de red adecuada para la carga de trabajo de servidor, el orden de las interfaces de red relacionados con la red los contadores de rendimiento y optimización del rendimiento de adaptadores de red y las tecnologías de red relacionadas, como Escala en lado de recepción \(RSS\), fusión de lado de recepción \(RSC\)y otros.

### <a name="nic-teamingtechnologiesnic-teamingnic-teamingmd"></a>[Formación de equipos NIC](technologies/nic-teaming/NIC-Teaming.md)

La formación de equipos NIC permite agrupar adaptadores de red Ethernet físicos en uno o más adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.

### <a name="quality-of-service-qos-policytechnologiesqosqos-policy-topmd"></a>[Calidad de servicio (QoS de) directiva](technologies/qos/qos-policy-top.md)

Puedes usar la directiva QoS como un punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuya configuración se distribuye con la directiva de grupo.

### <a name="remote-accessremoteremote-accessremote-accessmd"></a>[Acceso remoto](../remote/remote-access/remote-access.md)

Puede usar tecnologías de acceso remoto, como DirectAccess y redes privadas virtuales \(VPN\) para proporcionar a los trabajadores remotos con conectividad a los recursos de red interna. Además, puede usar acceso remoto para la red de área local \(LAN\) enrutamiento y para el Proxy de aplicación Web. que proporciona funcionalidad de proxy inversa para aplicaciones web dentro de la red corporativa, para permitir a los usuarios de cualquier dispositivo tener acceso a ellas desde fuera de la red corporativa.

La documentación de acceso remoto se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016. Para obtener más información, consulta [Acceso remoto](../remote/remote-access/remote-access.md).

Para obtener más información sobre los proxy de aplicación web, que es un servicio de rol del rol de servidor de acceso remoto, consulta [Proxy de aplicación web en Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### <a name="virtual-private-networking-vpnremoteremote-accessvpnvpn-topmd"></a>[Red privada virtual (VPN)](../remote/remote-access/vpn/vpn-top.md)

En Windows Server 2016, **DirectAccess y VPN** es un servicio de rol del rol de servidor de **Acceso remoto**.

Al instalar acceso remoto como un servidor VPN, puede usar redes privadas virtuales \(VPN\) para proporcionar los empleados remotos con conexiones de red de su organización a través de Internet - manteniendo también la información privacidad con conexiones cifradas.

Con la VPN de acceso remoto en Windows Server 2016 y los ordenadores cliente de Windows 10, ahora puedes implementar una VPN siempre activada. La VPN siempre activada te ofrece la posibilidad de administrar clientes de VPN remotos que siempre están conectados, proporcionando también a la vez comodidad para los trabajadores remotos, que ya no necesitan conectarse a la VPN de la red de la organización y desconectarse de ella.

Para obtener más información, consulta [Guía de implementación de VPN siempre activada para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentación de VPN se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016, en [Acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Para obtener más información sobre VPN, consulta [Redes virtuales privadas (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### <a name="windows-container-networkinghttpsdocsmicrosoftcomvirtualizationwindowscontainersmanage-containerscontainer-networking"></a>[Redes de contenedor de Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Redes de contenedores de Windows te permite crear y administrar redes para conectar terminales de contenedores en hosts de Windows 10 y Windows Server, usando herramientas y flujos de trabajo estándares del sector. Las redes de contenedores de Windows admiten varias topologías, incluyendo la privada, la plana L2 y la enrutada L3.

También se admiten son superpuestas que se pueden crear localmente en el host mediante el uso de Docker, Kubernetes o Windows PowerShell a través de complementos que se comunican con el servicio de red de Host de Windows \(SNP\). Puede crear y administrar varios\-redes de clústeres de nodo a través de los sistemas de orquestación de nivel superior mediante la comunicación a través de un agente local para SNP de cada nodo.

### <a name="windows-internet-name-service-winstechnologieswinswins-topmd"></a>[Servicio de nombres Internet de Windows (WINS)](technologies/wins/wins-top.md)

El servicio WINS es un servicio heredado de registro y resolución de nombres de ordenadores que asigna nombres NetBIOS de ordenador a direcciones IP. Se recomienda el uso de DNS frente al uso de WINS.

## <a name="additional-resources"></a>Recursos adicionales

Los recursos de redes para sistemas operativos anteriores a Windows Server 2016 se encuentran disponibles en las siguientes ubicaciones.

- [Introducción a las redes](https://technet.microsoft.com/library/hh831357.aspx) de Windows Server 2012 y Windows Server 2012 R2
- [Redes](https://technet.microsoft.com/library/cc753940) de Windows Server 2008 y Windows Server 2008 R2
- Windows Server 2003 [Contenido retirado R2 de Windows Server 2003/2003 ](https://www.microsoft.com/download/details.aspx?id=53314)
