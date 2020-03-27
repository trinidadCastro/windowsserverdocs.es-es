---
title: Funciones de red
description: Este tema proporciona una visión general de las tecnologías de redes definidas por software y plataforma de redes que están disponibles en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.openlocfilehash: 833f1681af16b4e79a28383462a3471b3bc08f52
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319366"
---
# <a name="networking"></a>Funciones de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

       [!TIP]
        Looking for information about older versions of Windows Server? Check out our other [Windows Server libraries](/previous-versions/windows/) on docs.microsoft.com. You can also [search this site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) for specific information.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> Las funciones de red son una parte fundamental del centro de recursos definido por software \(la plataforma de SDDC\), y Windows Server 2016 proporciona funciones de red definidas por software nuevas y mejoradas \(SDN\) Technologies para ayudarle a pasar a una solución de SDDC completa para su organización.

Al administrar redes como un recurso definido por software, puede describir los requisitos de infraestructura de una aplicación una vez y, después, elegir dónde se ejecuta la aplicación en el entorno local o en la nube. 

Esta coherencia significa que las aplicaciones son ahora más fáciles de escalar y que puedes ejecutar sin problemas las aplicaciones, en cualquier lugar, con la misma confianza en lo que respecta a la seguridad, el rendimiento, la calidad del servicio y la disponibilidad.

>[!Note]
> Para descargar Windows Server, consulta [Evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

Windows Server 2016 agrega las nuevas tecnologías de redes que se indican a continuación:

- Redes definidas por software: la controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma. La controladora de red le permite usar la virtualización de función de red para implementar fácilmente máquinas virtuales \(máquinas virtuales\) para el equilibrio de carga de software \(SLB\) para optimizar las cargas de tráfico de red para los inquilinos, y las puertas de enlace de RAS para proporcionar a los inquilinos las opciones de conectividad que necesitan entre los recursos locales y en la nube de Internet. También puede usar la controladora de red para administrar Datacenter Firewall en hosts de Hyper-V y máquinas virtuales.

- Plataforma de red: usar nuevas características para las tecnologías de plataforma de red existentes, puede usar la Directiva de DNS para personalizar las respuestas del servidor DNS a las consultas, usar una NIC convergente que controle el acceso directo a memoria remota combinada \(RDMA\) y el tráfico Ethernet, usar switch Embedded Teaming \(establecer\) para crear conmutadores virtuales de Hyper-V conectados a NIC RDMA y usar la administración de direcciones IP \(IPAM\) para administrar zonas y servidores DNS, así como

Para obtener más información, consulta [Escenarios de redes admitidos en Windows Server](windows-server-supported-networking-scenarios.md).

Las secciones siguientes proporcionan información sobre tecnologías SDN y tecnologías de plataformas de red.

## <a name="software-defined-networking-technologies"></a>Tecnologías de redes definidas por software

### <a name="software-defined-networking-40sdn41"></a>[Redes &#40;definidas por software Sdn&#41;](sdn/software-defined-networking.md)

Puede usar este tema para obtener información sobre las tecnologías de SDN que se proporcionan en Windows Server, System Center y Microsoft Azure.

>[!NOTE]
>En el caso de hosts de Hyper-V y máquinas virtuales \(máquinas virtuales\) que ejecutan servidores de infraestructura de SDN, como los nodos de equilibrio de carga de software y controladora de red, debe instalar Windows Server 2016 Datacenter Edition. En el caso de los hosts de Hyper-V que contienen solo máquinas virtuales de carga de trabajo de inquilino que están conectadas a SDN\-redes controladas, puede ejecutar Windows Server 2016 Standard Edition.

### <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>[Implementar una infraestructura de red definida por software mediante scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
En esta guía se proporcionan instrucciones sobre cómo implementar una controladora de red con redes virtuales y puertas de enlace en un entorno de laboratorio de pruebas.

### <a name="network-controller"></a>[Controladora de red](sdn/technologies/network-controller/Network-Controller.md)

La controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma.

### <a name="software-load-balancing-40slb41-for-sdn"></a>[SLB&#41; de equilibrio &#40;de carga de software para Sdn](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

Los proveedores de servicios en la nube \(CSP\) y las empresas que implementan redes definidas por software (SDN) en Windows Server 2016 pueden usar el equilibrio de carga de software \(SLB\) para distribuir uniformemente el tráfico de red del inquilino y del cliente del inquilino entre los recursos de red virtual. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.

### <a name="ras-gateway-for-sdn"></a>[Puerta de enlace RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

La puerta de enlace de RAS, que es un enrutador basado en software, multiinquilino, Protocolo de puerta de enlace de borde \(\) BGP en Windows Server 2016, está diseñada para proveedores de servicios en la nube \(CSP\) y empresas que hospedan varias redes virtuales de inquilinos con virtualización de red de Hyper-V. 

### <a name="network-function-virtualization"></a>[Virtualización de función de red](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

En los centros de recursos definidos por software, las funciones de red que realizan los dispositivos de hardware \(como equilibradores de carga, firewalls, enrutadores, conmutadores, etc.\) se virtualizan cada vez más como aplicaciones virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red.

### <a name="datacenter-firewall-overview"></a>[Información general de Datacenter Firewall](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Datacenter Firewall es un firewall multiinquilino con estado, de nivel de red y 5-tupla (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino).

## <a name="networking-technologies"></a><a name="bkmk_networking"></a>Tecnologías de red

En la tabla siguiente se proporcionan vínculos a algunas de las tecnologías de redes de Windows Server 2016.

### <a name="whats-new-in-networking"></a>[Novedades de redes](../networking/What-s-New-in-Networking.md)

Puedes usar las siguientes secciones para descubrir nuevas tecnologías de redes y nuevas funciones para tecnologías existentes en Windows Server 2016.

### <a name="branchcache"></a>[BranchCache](branchcache/BranchCache.md)

BranchCache es una red de área extensa \(tecnología de optimización de ancho de banda\) WAN. Para mejorar el ancho de banda de la WAN cuando los usuarios acceden a contenido de servidores remotos, BranchCache obtiene el contenido de los servidores de contenido de la oficina central o de la nube hospedada y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales accedan al contenido de forma local, y no a través de la WAN.

### <a name="core-network-guide-for-windows-server-2016"></a>[Guía de red principal para Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Aprende a implementar una red de Windows Server con la guía de red principal, así como a agregar funciones a la implementación de tu red con guías de complemento de red principal.

### <a name="directaccess"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess permite la conectividad de los usuarios remotos para los recursos de red de la organización. 

La documentación de DirectAccess se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016, en [Acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Para obtener más información, consulta [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### <a name="domain-name-system-40dns41"></a>[DNS del sistema &#40;de nombres de dominio&#41;](dns/dns-top.md)

El sistema de nombres de dominio \(DNS\) es una serie de protocolos estándar del sector que incluyen TCP/IP. De manera conjunta, el cliente DNS y el servidor DNS proporcionan servicios de resolución de nombres de asignación de nombres de equipo a direcciones IP para equipos y usuarios.

### <a name="dynamic-host-configuration-protocol-40dhcp41"></a>[Protocolo &#40;de configuración dinámica de host DHCP&#41;](technologies/dhcp/dhcp-top.md)

El protocolo de configuración dinámica de host \(DHCP\) es un protocolo cliente/servidor que proporciona automáticamente un host de protocolo de Internet \(IP\) con su dirección IP y otra información de configuración relacionada, como la máscara de subred y la puerta de enlace de predeterminada.

### <a name="hyper-v-network-virtualization"></a>[Virtualización de red de Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualización de red de Hyper-V \(HNV\) permite la virtualización de redes de clientes sobre una infraestructura de red física compartida "" ".

### <a name="hyper-v-virtual-switch"></a>[Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

El conmutador virtual de Hyper-V es un conmutador de red Ethernet de nivel 2 basado en software que está disponible en el Administrador de Hyper-V cuando se instala el rol de servidor de Hyper-V. El conmutador incluye mediante programación funcionalidades extensibles y administradas para conectar máquinas virtuales a la red física y a redes virtuales. Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad. 

La documentación del conmutador Virtual de Hyper-V se encuentra ahora en la sección **Virtualización** del índice de Windows Server 2016. Para obtener más información, consulte [Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="ip-address-management-40ipam41"></a>[Administración &#40;de direcciones IP IPAM&#41;](technologies/ipam/ipam-top.md)

La administración de direcciones IP \(IPAM\) es un conjunto de herramientas integrado que permite el planeamiento, la implementación, la administración y la supervisión de un extremo a otro de la infraestructura de direcciones IP, con una experiencia de usuario avanzada. IPAM detecta automáticamente los servidores de infraestructura de direcciones IP y el sistema de nombres de dominio \(servidores DNS\) en la red y permite administrarlos desde una interfaz central.

### <a name="network-load-balancing"></a>[Equilibrio de carga de red](technologies/Network-Load-Balancing.md)

El equilibrio de carga de red \(NLB\) distribuye el tráfico entre varios servidores mediante el protocolo de red TCP/IP. En el caso de implementaciones que no son de SDN, NLB garantiza que las aplicaciones sin estado, como un servidor web que ejecuta Internet Information Services \(IIS\), sean escalables al agregar más servidores a medida que aumenta la carga.

### <a name="high-performance-networking"></a>[Redes de alto rendimiento](technologies/hpn/hpn-top.md)

Las tecnologías de descarga de la red y optimización en Windows Server 2016 incluyen funciones y tecnologías de solo software (SO), funciones integradas de software y hardware (SH) y funciones y tecnologías de solo Hardware (HO).

La siguiente documentación de tecnología de descarga y optimización también está disponible.

- [Guía de configuración de la tarjeta de interfaz de red (NIC) convergente](technologies/conv-nic/cnic-top.md)
- [Protocolo de puente del centro de datos (DCB)](technologies/dcb/dcb-top.md)
- [Ajuste de escala en lado de recepción virtual (vRSS)](technologies/vrss/vrss-top.md)


### <a name="network-policy-server"></a>[Servidor de directivas de redes](technologies/nps/nps-top.md)

Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.

### <a name="network-shell-netsh"></a>[Shell de red (Netsh)](technologies/netsh/netsh.md)

Puede usar el shell de red \(utilidad de red netsh\) para administrar las tecnologías de red en Windows Server 2016 y Windows 10.

### <a name="network-subsystem-performance-tuning"></a>[Ajuste del rendimiento del subsistema de red](technologies/network-subsystem/net-sub-performance-top.md)

En este tema se proporciona información acerca de cómo elegir el adaptador de red adecuado para la carga de trabajo del servidor, las interfaces de red de pedidos, los contadores de rendimiento relacionados con la red y los adaptadores de red de ajuste del rendimiento y las tecnologías de red relacionadas, como el ajuste de escala en lado de recepción \(RSS\), la Unión al lado de recepción \(\)RSC y otros

### <a name="nic-teaming"></a>[Formación de equipos NIC](technologies/nic-teaming/NIC-Teaming.md)

La formación de equipos NIC permite agrupar adaptadores de red Ethernet físicos en uno o más adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.

### <a name="quality-of-service-qos-policy"></a>[Directiva de calidad de servicio (QoS)](technologies/qos/qos-policy-top.md)

Puedes usar la directiva QoS como un punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuya configuración se distribuye con la directiva de grupo.

### <a name="remote-access"></a>[Acceso remoto](../remote/remote-access/remote-access.md)

Puede usar tecnologías de acceso remoto, como DirectAccess y redes privadas virtuales \(VPN\) para proporcionar a los trabajadores remotos conectividad a los recursos de la red interna. Además, puede usar el acceso remoto para la red de área local \(el enrutamiento de\) LAN y para el proxy de aplicación Web. que proporciona funcionalidad de proxy inversa para aplicaciones web dentro de la red corporativa, para permitir a los usuarios de cualquier dispositivo tener acceso a ellas desde fuera de la red corporativa.

La documentación de acceso remoto se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016. Para obtener más información, consulta [Acceso remoto](../remote/remote-access/remote-access.md).

Para obtener más información sobre los proxy de aplicación web, que es un servicio de rol del rol de servidor de acceso remoto, consulta [Proxy de aplicación web en Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### <a name="virtual-private-networking-vpn"></a>[Redes privadas virtuales (VPN)](../remote/remote-access/vpn/vpn-top.md)

En Windows Server 2016, **DirectAccess y VPN** es un servicio de rol del rol de servidor de **Acceso remoto**.

Al instalar el acceso remoto como un servidor VPN, puede usar redes privadas virtuales \(VPN\) para proporcionar a los empleados remotos conexiones a la red de la organización a través de Internet, al tiempo que mantiene la privacidad de la información con conexiones cifradas.

Con la VPN de acceso remoto en Windows Server 2016 y los ordenadores cliente de Windows 10, ahora puedes implementar una VPN siempre activada. La VPN siempre activada te ofrece la posibilidad de administrar clientes de VPN remotos que siempre están conectados, proporcionando también a la vez comodidad para los trabajadores remotos, que ya no necesitan conectarse a la VPN de la red de la organización y desconectarse de ella.

Para obtener más información, consulta [Guía de implementación de VPN siempre activada para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentación de VPN se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016, en [Acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Para obtener más información sobre VPN, consulta [Redes virtuales privadas (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### <a name="windows-container-networking"></a>[Red de contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Redes de contenedores de Windows te permite crear y administrar redes para conectar terminales de contenedores en hosts de Windows 10 y Windows Server, usando herramientas y flujos de trabajo estándares del sector. Las redes de contenedores de Windows admiten varias topologías, incluyendo la privada, la plana L2 y la enrutada L3.

También se admiten las superposiciones que se pueden crear localmente en el host mediante Docker, Kubernetes o Windows PowerShell a través de complementos que se comunican con el servicio de red de host de Windows \(\)SNP. Puede crear y administrar redes de clúster de varios\-nodos a través de sistemas de orquestación de nivel superior mediante la comunicación a través de un agente local con el SNP de cada nodo.

### <a name="windows-internet-name-service-wins"></a>[Servicio de nombres Internet de Windows (WINS)](technologies/wins/wins-top.md)

El servicio WINS es un servicio heredado de registro y resolución de nombres de ordenadores que asigna nombres NetBIOS de ordenador a direcciones IP. Se recomienda el uso de DNS frente al uso de WINS.

## <a name="additional-resources"></a>Recursos adicionales

Los recursos de redes para sistemas operativos anteriores a Windows Server 2016 se encuentran disponibles en las siguientes ubicaciones.

- [Introducción a las redes](https://technet.microsoft.com/library/hh831357.aspx) de Windows Server 2012 y Windows Server 2012 R2
- [Redes](https://technet.microsoft.com/library/cc753940) de Windows Server 2008 y Windows Server 2008 R2
- Contenido retirado de Windows Server 2003 [Windows server 2003/2003 R2](https://www.microsoft.com/download/details.aspx?id=53314)
