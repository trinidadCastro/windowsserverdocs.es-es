---
title: Redes
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066849"
---
# Redes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> Las redes son una parte fundamental de la plataforma de Centro de datos definido por software \(SDDC\) y Windows Server 2016 proporciona tecnologías nuevas y mejoradas de redes definidas por software \(SDN\) para ayudarte a pasar a una solución SDDC realizada por completo para tu organización.

Al administrar las redes como un recurso definido por software, puede describir una vez los requisitos de infraestructura de la aplicación y, después, elegir dónde se ejecuta la aplicación: localmente o en la nube. 

Esta coherencia significa que las aplicaciones son ahora más fáciles de escalar y que puedes ejecutar sin problemas las aplicaciones, en cualquier lugar, con la misma confianza en lo que respecta a la seguridad, el rendimiento, la calidad del servicio y la disponibilidad.

>[!Note]
> Para descargar Windows Server, consulta [Evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

Windows Server 2016 agrega las nuevas tecnologías de redes que se indican a continuación:

- Redes definidas por software: la controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma. La controladora de red permite usar la virtualización de función de red para implementar fácilmente máquinas virtuales \(VM\) para el equilibrio de carga de software \(SLB\) para optimizar las cargas de tráfico de red para los inquilinos, así como puertas de enlace RAS para proporcionar a los inquilinos las opciones de conectividad que necesitan entre recursos de Internet, locales y en la nube. También puede usar la controladora de red para administrar Datacenter Firewall en hosts de Hyper-V y máquinas virtuales.

- Plataforma de red: mediante las nuevas funciones para las tecnologías de plataforma de red existentes, puedes usar una directiva DNS para personalizar las respuestas del servidor DNS a las consultas, usar una NIC convergida que controle el acceso directo a memoria remota \(RDMA\) y el tráfico de Ethernet combinados, usar Switch Embedded Teaming \(SET\) para crear conmutadores virtuales de Hyper-V conectados a NIC de RDMA y usar Administración de direcciones IP \(IPAM\) para administrar servidores y zonas DNS, así como direcciones IP y DHCP.

Para obtener más información, consulta [Escenarios de redes admitidos en Windows Server](windows-server-supported-networking-scenarios.md).

Las secciones siguientes proporcionan información sobre tecnologías SDN y tecnologías de plataformas de red.

## Tecnologías de redes definidas por software

### [Redes definidas por software &#40;SDN&#41;](sdn/software-defined-networking.md)

Puede usar este tema para obtener información sobre las tecnologías de SDN que se proporcionan en Windows Server, System Center y Microsoft Azure.

>[!NOTE]
>Para los hosts de Hyper-V y las máquinas virtuales \(VM\) que se ejecutan en los servidores de infraestructura SDN, como nodos de controlador de red y de equilibrio de carga de software, debes instalar la edición Windows Server 2016 Datacenter. Para los hosts de Hyper-V que contengan solo VM de carga de trabajo de inquilinos que estén conectados a redes controladas por SDN\, puedes ejecutar la edición Windows Server 2016 Standard.

### [Implementar una infraestructura de red definida por software con scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
En esta guía se proporcionan instrucciones sobre cómo implementar una controladora de red con redes virtuales y puertas de enlace en un entorno de laboratorio de pruebas.

### [Controladora de red](sdn/technologies/network-controller/Network-Controller.md)

La controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma.

### [Equilibrio de carga de software &#40;SLB&#41; para SDN](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

Los proveedores de servicios en la nube \(CSPs\) y las empresas que implementan redes definidas por software (SDN) en Windows Server 2016 pueden usar el equilibrio de carga de software \(SLB\) para distribuir de manera uniforme el tráfico de red de inquilinos y de clientes de inquilinos entre los recursos de la red virtual. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.

### [Puerta de enlace RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

La puerta de enlace RAS, que es un enrutador multiinquilino basado en software compatible con el Protocolo de puerta de enlace de borde \(BGP\) de Windows Server 2016, está diseñada para los proveedores de servicios en la nube \(CSP\) y las empresas que alojan varias redes virtuales de inquilinos mediante Virtualización de red de Hyper-V. 

### [Virtualización de función de red](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

En los centros de datos definidos por software, las funciones de red que se llevan a cabo mediante dispositivos hardware \(por ejemplo, equilibradores de carga, firewalls, enrutadores, conmutadores, etc.\) se están virtualizando cada vez más como dispositivos virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red.

### [Información general de Datacenter Firewall](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Datacenter Firewall es un firewall multiinquilino con estado, de nivel de red y 5-tupla (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino).

## <a name="bkmk_networking"></a>Tecnologías de redes

En la tabla siguiente se proporcionan vínculos a algunas de las tecnologías de redes de Windows Server 2016.

### [Novedades de redes](../networking/What-s-New-in-Networking.md)

Puedes usar las siguientes secciones para descubrir nuevas tecnologías de redes y nuevas funciones para tecnologías existentes en Windows Server 2016.

### [BranchCache](branchcache/BranchCache.md)

BranchCache es una tecnología de optimización de ancho de banda de red de área extensa \(WAN\). Para mejorar el ancho de banda de la WAN cuando los usuarios acceden a contenido de servidores remotos, BranchCache obtiene el contenido de los servidores de contenido de la oficina central o de la nube hospedada y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales accedan al contenido de forma local, y no a través de la WAN.

### [Guía de red principal para Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Aprende a implementar una red de Windows Server con la guía de red principal, así como a agregar funciones a la implementación de tu red con guías de complemento de red principal.

### [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess permite la conectividad de los usuarios remotos para los recursos de red de la organización. 

La documentación de DirectAccess se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016, en [Acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Para obtener más información, consulta [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### [Sistema de nombres de dominio &#40;DNS&#41;](dns/dns-top.md)

El sistema de nombres de dominio \(DNS\) es una de las series de protocolos estándar del sector que incluyen TCP/IP y el cliente DNS y el servidor DNS proporcionan de manera conjunta servicios de resolución de nombres de asignación de nombres de equipo a direcciones IP para ordenadores y usuarios.

### [Protocolo de configuración dinámica de host &#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

El protocolo de configuración dinámica de host \(DHCP\) es un protocolo cliente/servidor que proporciona automáticamente un host de protocolo de Internet \(IP\) con su dirección IP y otra información de configuración relacionada, como la máscara de subred y la puerta de enlace predeterminada.

### [Virtualización de red de Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualización de red de Hyper-V \(HNV\) permite la virtualización de redes de cliente en la parte superior de una infraestructura de red física compartida.

### [Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

El conmutador virtual de Hyper-V es un conmutador de red Ethernet de nivel 2 basado en software que está disponible en el Administrador de Hyper-V cuando se instala el rol de servidor de Hyper-V. El conmutador incluye mediante programación funcionalidades extensibles y administradas para conectar máquinas virtuales a la red física y a redes virtuales. Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad. 

La documentación del conmutador Virtual de Hyper-V se encuentra ahora en la sección **Virtualización** del índice de Windows Server 2016. Para obtener más información, consulte [Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### [Administración de direcciones IP &#40;IPAM&#41;](technologies/ipam/ipam-top.md)

La administración de direcciones IP \(IPAM\) es un conjunto de herramientas integrado que permite el planeamiento, la implementación, la administración y la supervisión de extremo a extremo de la infraestructura de direcciones IP, con una experiencia de usuario enriquecida. IPAM detecta automáticamente los servidores con infraestructura de direcciones IP y los servidores del Sistema de nombres de dominio \(DNS\) de la red y permite administrarlos desde una interfaz central.

### [Equilibrio de carga de red](technologies/Network-Load-Balancing.md)

El equilibrio de carga de red \(NLB\) distribuye el tráfico entre varios servidores mediante el protocolo de red TCP/IP. En el caso de implementaciones que no son de SDN, NLB garantiza que las aplicaciones sin estado, como los servidores web que ejecutan Internet Information Services \(IIS\), sean escalables agregando más servidores a medida que aumenta la carga.

### [Redes de alto rendimiento](technologies/hpn/hpn-top.md)

Las tecnologías de descarga de la red y optimización en Windows Server 2016 incluyen funciones y tecnologías de solo software (SO), funciones integradas de software y hardware (SH) y funciones y tecnologías de solo Hardware (HO).

La siguiente documentación de tecnología de descarga y optimización también está disponible.

- [Guía de configuración de tarjeta de interfaz de red (NIC) convergida](technologies/conv-nic/cnic-top.md)
- [Protocolo de puente del centro de datos (DCB)](technologies/dcb/dcb-top.md)
- [Ajuste de escala en lado de recepción virtual (vRSS)](technologies/vrss/vrss-top.md)


### [Servidor de directivas de redes](technologies/nps/nps-top.md)

Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.

### [Shell de red (Netsh)](technologies/netsh/netsh.md)

Puedes usar la utilidad de red Shell de red \(netsh\) para administrar las tecnologías de redes en Windows Server 2016 y Windows 10.

### [Ajuste del rendimiento del subsistema de red](technologies/network-subsystem/net-sub-performance-top.md)

Este tema proporciona información sobre cómo elegir el adaptador de red apropiado para la carga de trabajo del servidor, la petición de interfaces de red, los contadores de rendimiento relacionados con la red y el ajuste de rendimiento de los adaptadores de red y tecnologías de red relacionadas, como el ajuste de escala en el lado de recepción \(RSS\), Receive Side Coalescing \(RSC\) y otras.

### [Formación de equipos NIC](technologies/nic-teaming/NIC-Teaming.md)

La formación de equipos NIC permite agrupar adaptadores de red Ethernet físicos en uno o más adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.

### [Directiva de calidad de servicio (QoS)](technologies/qos/qos-policy-top.md)

Puedes usar la directiva QoS como un punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuya configuración se distribuye con la directiva de grupo.

### [Acceso remoto](../remote/remote-access/remote-access.md)

Puedes usar tecnologías de acceso remoto, como DirectAccess y redes virtuales privadas \(VPN\) para proporcionar a los trabajadores remotos conectividad a los recursos de la red interna. Además, puedes usar el acceso remoto para el enrutamiento de red de área local \(LAN\) y para el proxy de aplicación web. que proporciona funcionalidad de proxy inversa para aplicaciones web dentro de la red corporativa, para permitir a los usuarios de cualquier dispositivo tener acceso a ellas desde fuera de la red corporativa.

La documentación de acceso remoto se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016. Para obtener más información, consulta [Acceso remoto](../remote/remote-access/remote-access.md).

Para obtener más información sobre los proxy de aplicación web, que es un servicio de rol del rol de servidor de acceso remoto, consulta [Proxy de aplicación web en Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### [Redes privadas virtuales (VPN)](../remote/remote-access/vpn/vpn-top.md)

En Windows Server 2016, **DirectAccess y VPN** es un servicio de rol del rol de servidor de **Acceso remoto**.

Cuando instalas acceso remoto como un servidor VPN, puedes usar las redes privadas virtuales \(VPN\) para proporcionar a los empleados remotos conexiones a la red de la organización a través de Internet, manteniendo también a la vez la privacidad de la información con conexiones cifradas.

Con la VPN de acceso remoto en Windows Server 2016 y los ordenadores cliente de Windows 10, ahora puedes implementar una VPN siempre activada. La VPN siempre activada te ofrece la posibilidad de administrar clientes de VPN remotos que siempre están conectados, proporcionando también a la vez comodidad para los trabajadores remotos, que ya no necesitan conectarse a la VPN de la red de la organización y desconectarse de ella.

Para obtener más información, consulta [Guía de implementación de VPN siempre activada para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentación de VPN se encuentra ahora en la sección [Administración de acceso y servidor remoto](https://docs.microsoft.com/windows-server/remote/) del índice de Windows Server 2016, en [Acceso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Para obtener más información sobre VPN, consulta [Redes virtuales privadas (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### [Red de contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Redes de contenedores de Windows te permite crear y administrar redes para conectar terminales de contenedores en hosts de Windows 10 y Windows Server, usando herramientas y flujos de trabajo estándares del sector. Las redes de contenedores de Windows admiten varias topologías, incluyendo la privada, la plana L2 y la enrutada L3.

También se admiten superposiciones que puedes crear localmente en el host usando Docker, Kubernetes o Windows PowerShell a través de complementos que se comunican con el servicio de redes de host \(HNS\) de Windows. Puedes crear y administrar redes de clústeres de varios nodos a través de sistemas de organización de nivel superior mediante la comunicación a través de un agente local al HNS de cada nodo.

### [Servicio de nombres de Internet de Windows (WINS)](technologies/wins/wins-top.md)

El servicio WINS es un servicio heredado de registro y resolución de nombres de ordenadores que asigna nombres NetBIOS de ordenador a direcciones IP. Se recomienda el uso de DNS frente al uso de WINS.

## Recursos adicionales

Los recursos de redes para sistemas operativos anteriores a Windows Server 2016 se encuentran disponibles en las siguientes ubicaciones.

- [Introducción a las redes](https://technet.microsoft.com/library/hh831357.aspx) de Windows Server 2012 y Windows Server 2012 R2
- [Redes](https://technet.microsoft.com/library/cc753940) de Windows Server 2008 y Windows Server 2008 R2
- Windows Server 2003 [Contenido retirado R2 de Windows Server 2003/2003 ](https://www.microsoft.com/download/details.aspx?id=53314)
