---
title: Redes
description: En este tema se proporciona una visión general de las tecnologías de redes definidas por software y la plataforma de redes que están disponibles en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: anpaul
author: AnirbanPaul
ms.localizationpriority: medium
ms.openlocfilehash: 39bda1ac3a8b3cbac61435b65baf538f2d71e20e
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408914"
---
# <a name="networking"></a>Redes

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

> [!TIP]
> ¿Busca información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puede [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> Las funciones de red son una parte fundamental de la plataforma de servidor de bases de Datacenter de Datacenter \( \) y Windows Server 2016 proporciona tecnologías de red de redes definidas por software nuevas y mejoradas \( \) que le ayudarán a pasar a una solución de SDDC completa para su organización.

Al administrar redes como un recurso definido por software, puede describir los requisitos de infraestructura de una aplicación una vez y, después, elegir dónde se ejecuta la aplicación en el entorno local o en la nube.

Esta coherencia significa que las aplicaciones son ahora más fáciles de escalar y que puede ejecutar aplicaciones sin problemas con la misma confianza sobre la seguridad, el rendimiento, la calidad del servicio y la disponibilidad.

>[!Note]
> Para descargar Windows Server, consulte [evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

Windows Server 2016 agrega las nuevas tecnologías de redes que se indican a continuación:

- Redes definidas por software: la controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma. La controladora de red le permite usar la virtualización de función de red para implementar fácilmente máquinas virtuales de máquinas virtuales \( \) para el equilibrio de carga de software \( SLB con \) el fin de optimizar las cargas de tráfico de red para los inquilinos y las puertas de enlace de ras para proporcionar a los inquilinos las opciones de conectividad que necesitan entre los recursos locales y en la nube de Internet. También puede usar la controladora de red para administrar Datacenter Firewall en hosts de Hyper-V y máquinas virtuales.

- Plataforma de red: usar nuevas características para las tecnologías de plataforma de red existentes, puede usar la Directiva de DNS para personalizar las respuestas del servidor DNS a las consultas, usar una NIC convergente que controle el tráfico RDMA y Ethernet de acceso directo a memoria remota combinada \( \) , usar switch Embedded Teaming \( set \) para crear conmutadores virtuales de Hyper-V conectados a NIC RDMA y usar la administración de direcciones IP \( IPAM \) para administrar zonas y servidores DNS, así como direcciones DHCP y IP.

Para obtener más información, consulte [escenarios de redes compatibles con Windows Server](windows-server-supported-networking-scenarios.md).

En las secciones siguientes se proporciona información sobre las tecnologías de SDN y las tecnologías de plataforma de red.

## <a name="software-defined-networking-technologies"></a>Tecnologías de redes definidas por software

### <a name="software-defined-networking-40sdn41"></a>[Redes definidas por software &#40;SDN&#41;](sdn/software-defined-networking.md)

Puede usar este tema para obtener información sobre las tecnologías de SDN que se proporcionan en Windows Server, System Center y Microsoft Azure.

>[!NOTE]
>En el caso de los hosts de Hyper-V y máquinas virtuales \( \) que ejecutan servidores de infraestructura de Sdn, como los nodos de equilibrio de carga de software y controladora de red, debe instalar Windows Server 2016 Datacenter Edition. En el caso de los hosts de Hyper-V que contienen solo máquinas virtuales de carga de trabajo de inquilino que están conectadas a \- redes controladas por Sdn, puede ejecutar Windows Server 2016 Standard Edition.

### <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>[Implementación de una infraestructura de red definida por software con scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
En esta guía se proporcionan instrucciones sobre cómo implementar una controladora de red con redes virtuales y puertas de enlace en un entorno de laboratorio de pruebas.

### <a name="network-controller"></a>[Controladora de red](sdn/technologies/network-controller/Network-Controller.md)

La controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma.

### <a name="software-load-balancing-40slb41-for-sdn"></a>[Equilibrio de carga de software &#40;SLB&#41; para SDN](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

Los proveedores de servicios en la nube \( CSP \) y empresas que implementan redes definidas por software (SDN) en Windows Server 2016 pueden usar el SLB de equilibrio de carga de software \( \) para distribuir uniformemente el tráfico de red del inquilino y del cliente del inquilino entre los recursos de red virtual. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.

### <a name="ras-gateway-for-sdn"></a>[Puerta de enlace RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

La puerta de enlace de RAS, que es un enrutador basado en software, multiempresa Protocolo de puerta de enlace de borde \( \) compatible con BGP en Windows Server 2016, está diseñada para proveedores de servicios en la nube \( CSP \) y empresas que hospedan varias redes virtuales de inquilinos mediante virtualización de red de Hyper-V.

### <a name="network-function-virtualization"></a>[Virtualización de función de red](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

En los centros de recursos definidos por software, las funciones de red que realizan los dispositivos de hardware, como \( equilibradores de carga, firewalls, enrutadores, conmutadores, etc \) ., se virtualizan cada vez más como aplicaciones virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red.

### <a name="datacenter-firewall-overview"></a>[Información general de Datacenter Firewall](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Datacenter Firewall es un firewall multiinquilino con estado, de nivel de red y 5-tupla (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino).

## <a name="networking-technologies"></a><a name="bkmk_networking"></a>Tecnologías de redes

En la tabla siguiente se proporcionan vínculos a algunas de las tecnologías de red de Windows Server 2016.

### <a name="whats-new-in-networking"></a>[Novedades de redes](../networking/What-s-New-in-Networking.md)

Puede usar las siguientes secciones para descubrir nuevas tecnologías de red y nuevas características de las tecnologías existentes en Windows Server 2016.

### <a name="branchcache"></a>[BranchCache](branchcache/BranchCache.md)

BranchCache es una \( tecnología de optimización del ancho de banda WAN de red de área extensa \) . Para mejorar el ancho de banda de la WAN cuando los usuarios acceden a contenido de servidores remotos, BranchCache obtiene el contenido de los servidores de contenido de la oficina central o de la nube hospedada y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales accedan al contenido de forma local, y no a través de la WAN.

### <a name="core-network-guide-for-windows-server-2016"></a>[Guía de red principal para Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Aprenda a implementar una red de Windows Server con la guía de red principal, así como a agregar características a la implementación de red con las guías complementarias de red principal.

### <a name="directaccess"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess permite la conectividad de los usuarios remotos con los recursos de la red de la organización.

La documentación de DirectAccess se encuentra ahora en la sección [Administración de acceso remoto y administración de servidores](https://docs.microsoft.com/windows-server/remote/) de la tabla de contenido de Windows Server 2016, en [acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Para obtener más información, consulte [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### <a name="domain-name-system-40dns41"></a>[Sistema de nombres de dominio &#40;DNS&#41;](dns/dns-top.md)

El sistema de nombres de dominio \(DNS\) es una serie de protocolos estándar del sector que incluyen TCP/IP. De manera conjunta, el cliente DNS y el servidor DNS proporcionan servicios de resolución de nombres de asignación de nombres de equipo a direcciones IP para equipos y usuarios.

### <a name="dynamic-host-configuration-protocol-40dhcp41"></a>[Protocolo de configuración dinámica de host &#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

El protocolo de configuración dinámica de host \(DHCP\) es un protocolo cliente/servidor que proporciona automáticamente un host de protocolo de Internet \(IP\) con su dirección IP y otra información de configuración relacionada, como la máscara de subred y la puerta de enlace de predeterminada.

### <a name="hyper-v-network-virtualization"></a>[Virtualización de red de Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

La virtualización de red de Hyper-V \( HNV \) permite la virtualización de redes de clientes sobre una infraestructura de red física compartida "" ".

### <a name="hyper-v-virtual-switch"></a>[Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

El conmutador virtual de Hyper-V es un conmutador de red Ethernet de nivel 2 basado en software que está disponible en el Administrador de Hyper-V cuando se instala el rol de servidor de Hyper-V. El conmutador incluye mediante programación funcionalidades extensibles y administradas para conectar máquinas virtuales a la red física y a redes virtuales. Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad.

La documentación del conmutador virtual de Hyper-V se encuentra ahora en la sección **Virtualización** de la tabla de contenido de Windows Server 2016. Para obtener más información, consulte [conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="ip-address-management-40ipam41"></a>[Administración de direcciones IP &#40;IPAM&#41;](technologies/ipam/ipam-top.md)

La administración de direcciones IP \(IPAM\) es un conjunto de herramientas integrado que permite el planeamiento, la implementación, la administración y la supervisión de un extremo a otro de la infraestructura de direcciones IP, con una experiencia de usuario avanzada. IPAM detecta automáticamente los servidores de infraestructura de direcciones IP y \( \) los servidores DNS de sistema de nombres de dominio en la red y permite administrarlos desde una interfaz central.

### <a name="network-load-balancing"></a>[Equilibrio de carga de red](technologies/Network-Load-Balancing.md)

El equilibrio de carga de red \(NLB\) distribuye el tráfico entre varios servidores mediante el protocolo de red TCP/IP. En el caso de implementaciones que no son de SDN, NLB garantiza que las aplicaciones sin estado, como un servidor web que ejecuta Internet Information Services \(IIS\), sean escalables al agregar más servidores a medida que aumenta la carga.

### <a name="high-performance-networking"></a>[Redes de alto rendimiento](technologies/hpn/hpn-top.md)

Las tecnologías de descarga y optimización de red en Windows Server 2016 incluyen solo software (SO) características y tecnologías, características y tecnologías integradas de software y hardware (SH) y tecnologías y características de hardware únicamente (HO).

También está disponible la siguiente documentación sobre la tecnología de descarga y optimización.

- [Guía de configuración de la tarjeta de interfaz de red (NIC) convergente](technologies/conv-nic/cnic-top.md)
- [Protocolo de puente del centro de datos (DCB)](technologies/dcb/dcb-top.md)
- [Ajuste de escala en lado de recepción virtual (vRSS)](technologies/vrss/vrss-top.md)


### <a name="network-policy-server"></a>[Servidor de directivas de redes](technologies/nps/nps-top.md)

El servidor de directivas de redes (NPS) permite crear y aplicar directivas de acceso a la red de toda la organización para la autenticación y autorización de solicitudes de conexión.

### <a name="network-shell-netsh"></a>[Shell de red (Netsh)](technologies/netsh/netsh.md)

Puede usar la \( utilidad Netsh networking de Shell de red \) para administrar las tecnologías de red en windows Server 2016 y Windows 10.

### <a name="network-subsystem-performance-tuning"></a>[Ajuste del rendimiento del subsistema de red](technologies/network-subsystem/net-sub-performance-top.md)

En este tema se proporciona información acerca de cómo elegir el adaptador de red adecuado para la carga de trabajo del servidor, las interfaces de red de pedidos, los contadores de rendimiento relacionados con la red y los adaptadores de red de ajuste del rendimiento y las tecnologías de red relacionadas, como el ajuste de escala en lado de recepción \( \) de RSS, el RSC de fusión lateral \( \) y otros.

### <a name="nic-teaming"></a>[Formación de equipos NIC](technologies/nic-teaming/NIC-Teaming.md)

La formación de equipos NIC permite agrupar adaptadores de red Ethernet físicos en uno o más adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.

### <a name="quality-of-service-qos-policy"></a>[Directiva de calidad de servicio (QoS)](technologies/qos/qos-policy-top.md)

Puede usar la Directiva de QoS como punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuyos valores se distribuyen con directiva de grupo.

### <a name="remote-access"></a>[Acceso remoto](../remote/remote-access/remote-access.md)

Puede usar tecnologías de acceso remoto, como DirectAccess y VPN de red privada \( virtual \) para proporcionar a los trabajadores remotos conectividad a los recursos de la red interna. Además, puede usar el acceso remoto para el enrutamiento de LAN de red de área local \( \) y para el proxy de aplicación Web. que proporciona funcionalidad de proxy inverso para aplicaciones web dentro de la red corporativa para permitir a los usuarios de cualquier dispositivo tener acceso a ellas desde fuera de la red corporativa.

La documentación de acceso remoto se encuentra ahora en la sección [Administración de acceso remoto y administración de servidores](https://docs.microsoft.com/windows-server/remote/) de la tabla de contenido de Windows Server 2016. Para obtener más información, consulte [acceso remoto](../remote/remote-access/remote-access.md).

Para obtener más información sobre el proxy de aplicación Web, que es un servicio de rol del rol de servidor de acceso remoto, vea [proxy de aplicación web en Windows server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### <a name="virtual-private-networking-vpn"></a>[Redes privadas virtuales (VPN)](../remote/remote-access/vpn/vpn-top.md)

En Windows Server 2016, **DirectAccess y VPN** es un servicio de rol del rol de servidor de **acceso remoto** .

Al instalar el acceso remoto como un servidor VPN, puede usar la VPN de red privada virtual \( \) para proporcionar a los empleados remotos conexiones a la red de la organización a través de Internet, al tiempo que mantiene la privacidad de la información con conexiones cifradas.

Con Windows Server 2016 VPN de acceso remoto y equipos cliente de Windows 10, ahora puede implementar Always On VPN. Always On VPN le ofrece la capacidad de administrar clientes VPN remotos que siempre están conectados, a la vez que proporciona comodidad para los trabajadores remotos, que ya no necesitan conectarse y desconectarse manualmente de la VPN a la red de su organización.

Para obtener más información, consulte el [acceso remoto Always on guía de implementación de VPN para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentación de VPN se encuentra ahora en la sección [Administración de acceso remoto y administración de servidores](https://docs.microsoft.com/windows-server/remote/) de la tabla de contenido de Windows Server 2016, en [acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Para obtener más información acerca de VPN, consulte [red privada virtual (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### <a name="windows-container-networking"></a>[Red de contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Las redes de contenedor de Windows permiten crear y administrar redes para conectar puntos de conexión de contenedor en hosts de Windows 10 y Windows Server mediante el uso de las herramientas y los flujos de trabajo estándar del sector. Las redes de contenedor de Windows admiten varias topologías, como Private, Flat-L2 y enrutado-L3.

También se admiten las superposiciones que se pueden crear localmente en el host mediante Docker, Kubernetes o Windows PowerShell a través de complementos que se comunican con el servicio de red de host de Windows \( SNP \) . Puede crear y administrar redes de \- clúster de varios nodos a través de sistemas de orquestación de nivel superior mediante la comunicación a través de un agente local con el SNP de cada nodo.

### <a name="windows-internet-name-service-wins"></a>[Servicio de nombres Internet de Windows (WINS)](technologies/wins/wins-top.md)

El servicio de nombres Internet de Windows (WINS) es un servicio de resolución y registro de nombres de equipo heredado que asigna nombres NetBIOS de equipo a direcciones IP. Se recomienda usar DNS sobre el uso de WINS.

## <a name="additional-resources"></a>Recursos adicionales

Los recursos de red para los sistemas operativos anteriores a Windows Server 2016 están disponibles en las siguientes ubicaciones.

- [Introducción a las redes](https://technet.microsoft.com/library/hh831357.aspx) de Windows Server 2012 y Windows Server 2012 R2
- [Redes](https://technet.microsoft.com/library/cc753940) de Windows Server 2008 y Windows Server 2008 R2
- Contenido retirado de Windows Server 2003 [Windows server 2003/2003 R2](https://www.microsoft.com/download/details.aspx?id=53314)
