---
title: Funciones de red
description: Este tema proporciona una visión general de las tecnologías de redes definidas por software y plataforma de redes que están disponibles en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 1cc30f239f1c4c9107ce0afcfb70647d43384949
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356784"
---
# <a name="networking"></a>Funciones de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> La red es una \(parte fundamental de la\) plataforma de centro de centros de recursos definido por software, y Windows Server 2016 proporciona\) tecnologías de \(red de redes definidas por software nuevas y mejoradas que le ayudarán a pasar a una solución de SDDC completa para su organización.

Al administrar redes como un recurso definido por software, puede describir los requisitos de infraestructura de una aplicación una vez y, después, elegir dónde se ejecuta la aplicación en el entorno local o en la nube. 

Esta coherencia significa que las aplicaciones son ahora más fáciles de escalar y que puedes ejecutar sin problemas las aplicaciones, en cualquier lugar, con la misma confianza en lo que respecta a la seguridad, el rendimiento, la calidad del servicio y la disponibilidad.

>[!Note]
> Para descargar Windows Server, consulta [Evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

Windows Server 2016 agrega las nuevas tecnologías de redes que se indican a continuación:

- Redes definidas por software: La controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma. La controladora de red le permite usar la virtualización de función de red \(para\) implementar fácilmente máquinas virtuales \(de\) virtual machines para el equilibrio de carga de software SLB con el fin de optimizar las cargas de tráfico de red para los inquilinos y ras Puertas de enlace para proporcionar a los inquilinos las opciones de conectividad que necesitan entre los recursos locales y en la nube de Internet. También puede usar la controladora de red para administrar Datacenter Firewall en hosts de Hyper-V y máquinas virtuales.

- Plataforma de red: Con las nuevas características de las tecnologías de plataforma de red existentes, puede usar la Directiva de DNS para personalizar las respuestas del servidor DNS a las consultas, usar una NIC convergente que \(controle el tráfico RDMA\) y Ethernet de acceso directo a memoria remota combinada. , use switch Embedded Teaming \(set\) para crear conmutadores virtuales de Hyper-V conectados a NIC RDMA y use la administración \(de\) direcciones IP IPAM para administrar servidores y zonas DNS, así como direcciones DHCP y IP.

Para obtener más información, consulta [Escenarios de redes admitidos en Windows Server](windows-server-supported-networking-scenarios.md).

Las secciones siguientes proporcionan información sobre tecnologías SDN y tecnologías de plataformas de red.

## <a name="software-defined-networking-technologies"></a>Tecnologías de redes definidas por software

### <a name="software-defined-networking-40sdn41sdnsoftware-defined-networkingmd"></a>[Redes &#40;definidas por software Sdn&#41;](sdn/software-defined-networking.md)

Puede usar este tema para obtener información sobre las tecnologías de SDN que se proporcionan en Windows Server, System Center y Microsoft Azure.

>[!NOTE]
>En el caso de los hosts de \(Hyper\) -V y máquinas virtuales que ejecutan servidores de infraestructura de Sdn, como los nodos de equilibrio de carga de software y controladora de red, debe instalar Windows Server 2016 Datacenter Edition. En el caso de los hosts de Hyper-V que contienen solo máquinas virtuales\-de carga de trabajo de inquilino que están conectadas a redes controladas por Sdn, puede ejecutar Windows Server 2016 Standard Edition.

### <a name="deploy-a-software-defined-network-infrastructure-using-scriptssdndeploydeploy-a-software-defined-network-infrastructure-using-scriptsmd"></a>[Implementar una infraestructura de red definida por software mediante scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
En esta guía se proporcionan instrucciones sobre cómo implementar una controladora de red con redes virtuales y puertas de enlace en un entorno de laboratorio de pruebas.

### <a name="network-controllersdntechnologiesnetwork-controllernetwork-controllermd"></a>[Controladora de red](sdn/technologies/network-controller/Network-Controller.md)

La controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma.

### <a name="software-load-balancing-40slb41-for-sdnsdntechnologiesnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[SLB&#41; de equilibrio &#40;de carga de software para Sdn](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

\(Los proveedores de servicios\) en la nube CSP y empresas que están implementando redes definidas por software (SDN) en Windows Server 2016 \(pueden\) usar el equilibrio de carga de software SLB para distribuir uniformemente el inquilino y el inquilino tráfico de red del cliente entre los recursos de red virtual. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.

### <a name="ras-gateway-for-sdnsdntechnologiesnetwork-function-virtualizationras-gateway-for-sdnmd"></a>[Puerta de enlace RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

La puerta de enlace de Ras, que es un enrutador basado \(en\) software, multiempresa protocolo de puerta de enlace de borde compatible con BGP en Windows Server 2016, \(está\) diseñada para proveedores de servicios en la nube CSP y empresas que hospedan varias redes virtuales de inquilinos que usan virtualización de red de Hyper-V. 

### <a name="network-function-virtualizationsdntechnologiesnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualización de función de red](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

En los centros de recursos definidos por software, las funciones de red que realizan los \(dispositivos de hardware, como equilibradores de carga, firewalls, enrutadores,\) conmutadores, etc., se virtualizan cada vez más como aplicaciones virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red.

### <a name="datacenter-firewall-overviewsdntechnologiesnetwork-function-virtualizationdatacenter-firewall-overviewmd"></a>[Información general de Datacenter Firewall](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Datacenter Firewall es un firewall multiinquilino con estado, de nivel de red y 5-tupla (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino).

## <a name="bkmk_networking"></a>Tecnologías de red

En la tabla siguiente se proporcionan vínculos a algunas de las tecnologías de redes de Windows Server 2016.

### <a name="whats-new-in-networkingnetworkingwhat-s-new-in-networkingmd"></a>[Novedades de redes](../networking/What-s-New-in-Networking.md)

Puedes usar las siguientes secciones para descubrir nuevas tecnologías de redes y nuevas funciones para tecnologías existentes en Windows Server 2016.

### <a name="branchcachebranchcachebranchcachemd"></a>[BranchCache](branchcache/BranchCache.md)

BranchCache es una tecnología de optimización \(del\) ancho de banda WAN de red de área extensa. Para mejorar el ancho de banda de la WAN cuando los usuarios acceden a contenido de servidores remotos, BranchCache obtiene el contenido de los servidores de contenido de la oficina central o de la nube hospedada y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales accedan al contenido de forma local, y no a través de la WAN.

### <a name="core-network-guide-for-windows-server-2016core-network-guidecore-network-guide-windows-servermd"></a>[Guía de red principal para Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Aprende a implementar una red de Windows Server con la guía de red principal, así como a agregar funciones a la implementación de tu red con guías de complemento de red principal.

### <a name="directaccessremoteremote-accessdirectaccessdirectaccessmd"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess permite la conectividad de los usuarios remotos para los recursos de red de la organización. 

La documentación de DirectAccess se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016, en [Acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Para obtener más información, consulta [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### <a name="domain-name-system-40dns41dnsdns-topmd"></a>[DNS del sistema &#40;de nombres de dominio&#41;](dns/dns-top.md)

El sistema de nombres de dominio \(DNS\) es una serie de protocolos estándar del sector que incluyen TCP/IP. De manera conjunta, el cliente DNS y el servidor DNS proporcionan servicios de resolución de nombres de asignación de nombres de equipo a direcciones IP para equipos y usuarios.

### <a name="dynamic-host-configuration-protocol-40dhcp41technologiesdhcpdhcp-topmd"></a>[Protocolo &#40;de configuración dinámica de host DHCP&#41;](technologies/dhcp/dhcp-top.md)

El protocolo de configuración dinámica de host \(DHCP\) es un protocolo cliente/servidor que proporciona automáticamente un host de protocolo de Internet \(IP\) con su dirección IP y otra información de configuración relacionada, como la máscara de subred y la puerta de enlace de predeterminada.

### <a name="hyper-v-network-virtualizationsdntechnologieshyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualización de red de Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

La Virtualización de red de Hyper-V \(HNV\) permite la virtualización de redes de cliente en una infraestructura de red física compartida.

### <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

El conmutador virtual de Hyper-V es un conmutador de red Ethernet de nivel 2 basado en software que está disponible en el Administrador de Hyper-V cuando se instala el rol de servidor de Hyper-V. El conmutador incluye mediante programación funcionalidades extensibles y administradas para conectar máquinas virtuales a la red física y a redes virtuales. Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad. 

La documentación del conmutador Virtual de Hyper-V se encuentra ahora en la sección **Virtualización** del índice de Windows Server 2016. Para obtener más información, consulte [Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="ip-address-management-40ipam41technologiesipamipam-topmd"></a>[Administración &#40;de direcciones IP IPAM&#41;](technologies/ipam/ipam-top.md)

La administración de direcciones IP \(IPAM\) es un conjunto de herramientas integrado que permite el planeamiento, la implementación, la administración y la supervisión de un extremo a otro de la infraestructura de direcciones IP, con una experiencia de usuario avanzada. IPAM detecta automáticamente los servidores de infraestructura de direcciones IP y los servidores \(DNS\) de sistema de nombres de dominio en la red y permite administrarlos desde una interfaz central.

### <a name="network-load-balancingtechnologiesnetwork-load-balancingmd"></a>[Equilibrio de carga de red](technologies/Network-Load-Balancing.md)

El equilibrio de carga de red \(NLB\) distribuye el tráfico entre varios servidores mediante el protocolo de red TCP/IP. En el caso de implementaciones que no son de SDN, NLB garantiza que las aplicaciones sin estado, como un servidor web que ejecuta Internet Information Services \(IIS\), sean escalables al agregar más servidores a medida que aumenta la carga.

### <a name="high-performance-networkingtechnologieshpnhpn-topmd"></a>[Redes de alto rendimiento](technologies/hpn/hpn-top.md)

Las tecnologías de descarga de la red y optimización en Windows Server 2016 incluyen funciones y tecnologías de solo software (SO), funciones integradas de software y hardware (SH) y funciones y tecnologías de solo Hardware (HO).

La siguiente documentación de tecnología de descarga y optimización también está disponible.

- [Guía de configuración de la tarjeta de interfaz de red (NIC) convergente](technologies/conv-nic/cnic-top.md)
- [Protocolo de puente del centro de datos (DCB)](technologies/dcb/dcb-top.md)
- [Ajuste de escala en lado de recepción virtual (vRSS)](technologies/vrss/vrss-top.md)


### <a name="network-policy-servertechnologiesnpsnps-topmd"></a>[Servidor de directivas de redes](technologies/nps/nps-top.md)

Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.

### <a name="network-shell-netshtechnologiesnetshnetshmd"></a>[Shell de red (Netsh)](technologies/netsh/netsh.md)

Puede usar la utilidad Netsh \(\) networking de Shell de red para administrar las tecnologías de red en Windows Server 2016 y Windows 10.

### <a name="network-subsystem-performance-tuningtechnologiesnetwork-subsystemnet-sub-performance-topmd"></a>[Ajuste del rendimiento del subsistema de red](technologies/network-subsystem/net-sub-performance-top.md)

En este tema se proporciona información acerca de cómo elegir el adaptador de red adecuado para la carga de trabajo del servidor, las interfaces de red de pedidos, los contadores de rendimiento relacionados con la red y los adaptadores de red de ajuste del rendimiento y las tecnologías de red relacionadas, como El ajuste de \(escala\)en lado \(de recepción RSS,\)de recepción de fusión lateral de recepción y otros.

### <a name="nic-teamingtechnologiesnic-teamingnic-teamingmd"></a>[Formación de equipos NIC](technologies/nic-teaming/NIC-Teaming.md)

La formación de equipos NIC permite agrupar adaptadores de red Ethernet físicos en uno o más adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.

### <a name="quality-of-service-qos-policytechnologiesqosqos-policy-topmd"></a>[Directiva de calidad de servicio (QoS)](technologies/qos/qos-policy-top.md)

Puedes usar la directiva QoS como un punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuya configuración se distribuye con la directiva de grupo.

### <a name="remote-accessremoteremote-accessremote-accessmd"></a>[Acceso remoto](../remote/remote-access/remote-access.md)

Puede usar tecnologías de acceso remoto, como DirectAccess y VPN \(\) de red privada virtual para proporcionar a los trabajadores remotos conectividad a los recursos de la red interna. Además, puede usar el acceso remoto para el enrutamiento de LAN \(\) de red de área local y para el proxy de aplicación Web. que proporciona funcionalidad de proxy inversa para aplicaciones web dentro de la red corporativa, para permitir a los usuarios de cualquier dispositivo tener acceso a ellas desde fuera de la red corporativa.

La documentación de acceso remoto se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016. Para obtener más información, consulta [Acceso remoto](../remote/remote-access/remote-access.md).

Para obtener más información sobre los proxy de aplicación web, que es un servicio de rol del rol de servidor de acceso remoto, consulta [Proxy de aplicación web en Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### <a name="virtual-private-networking-vpnremoteremote-accessvpnvpn-topmd"></a>[Redes privadas virtuales (VPN)](../remote/remote-access/vpn/vpn-top.md)

En Windows Server 2016, **DirectAccess y VPN** es un servicio de rol del rol de servidor de **Acceso remoto**.

Al instalar el acceso remoto como un servidor VPN, puede usar la VPN \(\) de red privada virtual para proporcionar a los empleados remotos conexiones a la red de la organización a través de Internet, al tiempo que mantiene la información. privacidad con conexiones cifradas.

Con la VPN de acceso remoto en Windows Server 2016 y los ordenadores cliente de Windows 10, ahora puedes implementar una VPN siempre activada. La VPN siempre activada te ofrece la posibilidad de administrar clientes de VPN remotos que siempre están conectados, proporcionando también a la vez comodidad para los trabajadores remotos, que ya no necesitan conectarse a la VPN de la red de la organización y desconectarse de ella.

Para obtener más información, consulta [Guía de implementación de VPN siempre activada para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentación de VPN se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016, en [Acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Para obtener más información sobre VPN, consulta [Redes virtuales privadas (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### <a name="windows-container-networkinghttpsdocsmicrosoftcomvirtualizationwindowscontainersmanage-containerscontainer-networking"></a>[Redes de contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Redes de contenedores de Windows te permite crear y administrar redes para conectar terminales de contenedores en hosts de Windows 10 y Windows Server, usando herramientas y flujos de trabajo estándares del sector. Las redes de contenedores de Windows admiten varias topologías, incluyendo la privada, la plana L2 y la enrutada L3.

También se admiten las superposiciones que se pueden crear localmente en el host mediante Docker, Kubernetes o Windows PowerShell a través de complementos que se comunican con el \(servicio\)de red de host de Windows SNP. Puede crear y administrar redes de\-clúster de varios nodos a través de sistemas de orquestación de nivel superior mediante la comunicación a través de un agente local con el SNP de cada nodo.

### <a name="windows-internet-name-service-winstechnologieswinswins-topmd"></a>[Servicio de nombres Internet de Windows (WINS)](technologies/wins/wins-top.md)

El servicio WINS es un servicio heredado de registro y resolución de nombres de ordenadores que asigna nombres NetBIOS de ordenador a direcciones IP. Se recomienda el uso de DNS frente al uso de WINS.

## <a name="additional-resources"></a>Recursos adicionales

Los recursos de redes para sistemas operativos anteriores a Windows Server 2016 se encuentran disponibles en las siguientes ubicaciones.

- [Introducción a las redes](https://technet.microsoft.com/library/hh831357.aspx) de Windows Server 2012 y Windows Server 2012 R2
- [Redes](https://technet.microsoft.com/library/cc753940) de Windows Server 2008 y Windows Server 2008 R2
- Contenido retirado de Windows Server 2003 [Windows server 2003/2003 R2](https://www.microsoft.com/download/details.aspx?id=53314)
