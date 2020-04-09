---
title: Funciones de red
description: Este tema proporciona una visión general de las tecnologías de redes definidas por software y plataforma de redes que están disponibles en Windows Server 2016.
ms.prod: windows-server
layout: LandingPage
ms.technology: networking
ms.topic: landing-page
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: anpaul
author: AnirbanPaul
ms.localizationpriority: medium
ms.openlocfilehash: de1acf0a0ed233fdd7675da3bca63f7f16824a9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855808"
---
# <a name="networking"></a>Funciones de red

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<hr />

Las funciones de red son una parte fundamental del centro de recursos definido por software \(la plataforma de SDDC\), y Windows Server 2016 proporciona funciones de red definidas por software nuevas y mejoradas \(SDN\) Technologies para ayudarle a pasar a una solución de SDDC completa para su organización.

Al administrar redes como un recurso definido por software, puede describir los requisitos de infraestructura de una aplicación una vez y, después, elegir dónde se ejecuta la aplicación en el entorno local o en la nube. 

Esta coherencia significa que las aplicaciones son ahora más fáciles de escalar y que puedes ejecutar sin problemas las aplicaciones, en cualquier lugar, con la misma confianza en lo que respecta a la seguridad, el rendimiento, la calidad del servicio y la disponibilidad.

<hr />

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-whats-new.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h2><a href="../networking/What-s-New-in-Networking.md">Novedades&#39;de redes</a></h2>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<hr />

<h2>Redes definidas por software</h2>

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="https://docs.microsoft.com/windows-server/networking/sdn/">Redes definidas por software (SDN)</a></h3>
                        <hr />
                        <p>Puede usar este tema para obtener información sobre las tecnologías de SDN que se proporcionan en Windows Server, System Center y Microsoft Azure.</p>
                        <p><b>Nota:</b> En el caso de los hosts de Hyper-V y las máquinas virtuales (VM) que ejecutan servidores de infraestructura de SDN, como los nodos de equilibrio de carga de software y controladora de red, debe instalar Windows Server Datacenter Edition. En el caso de los hosts de Hyper-V que contienen solo máquinas virtuales de carga de trabajo de inquilino que están conectadas a redes controladas por SDN, puede ejecutar Windows Server Standard Edition.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3><a href="sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md">Implementar una infraestructura de red definida por software mediante scripts</a></h3>
                    <hr />
                    <p>En esta guía se proporcionan instrucciones sobre cómo implementar una controladora de red con redes virtuales y puertas de enlace en un entorno de laboratorio de pruebas.</p>
                </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-controller/Network-Controller.md">Controladora de red</a></h3>
                        <hr />
                        <p>La controladora de red ofrece un punto de automatización programable y centralizado para administrar, configurar y supervisar la infraestructura de red virtual y física de su centro de datos, así como para resolver problemas en la misma.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md">SLB&#41; de equilibrio &#40;de carga de software para Sdn</a></h3>
                        <hr />
                        <p>Los proveedores de servicios en la nube (CSP) y las empresas que implementan redes definidas por software (SDN) en Windows Server 2016 pueden usar el equilibrio de carga de software (SLB) para distribuir uniformemente el tráfico de red del inquilino y del cliente del inquilino entre los recursos de red virtual. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md">Puerta de enlace RAS para SDN</a></h3>
                        <hr />
                        <p>La puerta de enlace RAS, que es un enrutador compatible con el software, multiempresa, Protocolo de puerta de enlace de borde (BGP) en Windows Server 2016, está diseñada para proveedores de servicios en la nube (CSP) y empresas que hospedan varias redes virtuales de inquilinos mediante la red de Hyper-V. Virtualización.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md">Virtualización de función de red</a></h3>
                        <hr />
                        <p>En los centros de recursos definidos por software, las funciones de red que realizan los dispositivos de hardware (como equilibradores de carga, firewalls, enrutadores, conmutadores, etc.) se virtualizan cada vez más como aplicaciones virtuales. Esta &quot;de virtualización de función de red&quot; es una progresión natural de virtualización de servidores y virtualización de red.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md">Información general de Datacenter Firewall</a></h3>
                        <hr />
                        <p>Datacenter Firewall es un firewall multiinquilino con estado, de nivel de red y 5-tupla (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino).</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<hr />

## <a name="networking-technologies"></a><a name="bkmk_networking"></a>Tecnologías de red

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="branchcache/BranchCache.md">BranchCache</a></h3>
                        <hr />
                        <p>BranchCache es una tecnología de optimización de ancho de banda de red de área extensa (WAN). Para mejorar el ancho de banda de la WAN cuando los usuarios acceden a contenido de servidores remotos, BranchCache obtiene el contenido de los servidores de contenido de la oficina central o de la nube hospedada y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales accedan al contenido de forma local, y no a través de la WAN.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="core-network-guide/core-network-guide-windows-server.md">Guía de red principal</a></h3>
                        <hr />
                        <p>Aprende a implementar una red de Windows Server con la guía de red principal, así como a agregar funciones a la implementación de tu red con guías de complemento de red principal.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/directaccess/DirectAccess.md">DirectAccess</a></h3>
                        <hr />
                        <p>DirectAccess permite la conectividad de los usuarios remotos para los recursos de red de la organización. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
   <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="dns/dns-top.md">Sistema de nombres de dominio (DNS)</a></h3>
                        <hr />
                        <p>El sistema de nombres de dominio (DNS) es uno de los conjuntos de protocolos estándar del sector que componen TCP/IP y, conjuntamente, el cliente DNS y el servidor DNS proporcionan servicios de resolución de nombres de nombre de equipo a dirección IP para equipos y usuarios.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/dhcp/dhcp-top.md">Protocolo &#40;de configuración dinámica de host DHCP&#41;</a></h3>
                        <hr />
                        <p>El protocolo de configuración dinámica de host (DHCP) es un protocolo cliente/servidor que proporciona automáticamente un host de protocolo de Internet (IP) con su dirección IP y otra información de configuración relacionada, como la máscara de subred y la puerta de enlace predeterminada.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md">Virtualización de red de Hyper-V</a></h3>
                        <hr />
                        <p>La virtualización de red de Hyper-V (HNV) permite la virtualización de redes de clientes sobre una infraestructura de red física compartida.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md">Conmutador virtual de Hyper-V</a></h3>
                        <hr />
                        <p>El conmutador virtual de Hyper-V es un conmutador de red Ethernet de nivel 2 basado en software que está disponible en el Administrador de Hyper-V cuando se instala el rol de servidor de Hyper-V. El conmutador incluye mediante programación funcionalidades extensibles y administradas para conectar máquinas virtuales a la red física y a redes virtuales. Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/ipam/ipam-top.md">Administración &#40;de direcciones IP IPAM&#41;</a></h3>
                        <hr />
                        <p>La administración de direcciones IP (IPAM) es un conjunto de herramientas integrado que permite la planificación, implementación, administración y supervisión de un extremo a otro de la infraestructura de direcciones IP, con una experiencia de usuario enriquecida. IPAM detecta automáticamente los servidores con infraestructura de direcciones IP y los servidores del Sistema de nombres de dominio (DNS) de la red y permite administrarlos desde una interfaz central.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/Network-Load-Balancing.md">Equilibrio de carga de red</a></h3>
                        <hr />
                        <p>Equilibrio de carga de red (NLB) distribuye el tráfico entre varios servidores mediante el protocolo de red TCP/IP. En el caso de las implementaciones que no son de SDN, NLB garantiza que las aplicaciones sin estado, como los servidores web que ejecutan Internet Information Services (IIS), sean escalables agregando más servidores a medida que aumenta la carga.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/hpn/hpn-top.md">Redes de alto rendimiento</a></h3>
                        <hr />
                        <p>Las tecnologías de descarga de la red y optimización en Windows Server 2016 incluyen funciones y tecnologías de solo software (SO), funciones integradas de software y hardware (SH) y funciones y tecnologías de solo Hardware (HO).</p>
                        <p>La siguiente documentación de tecnología de descarga y optimización también está disponible:<p>
                        <hr />
                        <a href="technologies/conv-nic/cnic-top.md">Tarjeta de interfaz de red (NIC) convergente</a>
                        <hr />Protocolo de 
                        <a href="technologies/dcb/dcb-top.md">puente del centro de datos (DCB)</a>
                        <hr />
                     de 
                        <a href="technologies/vrss/vrss-top.md">ajuste de escala en lado de recepción virtual (vRSS)</a></div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/nps/nps-top.md">Servidor de directivas de redes</a></h3>
                        <hr />
                        <p>Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/netsh/netsh.md">Shell de red (Netsh)</a></h3>
                        <hr />
                        <p>Puede usar la utilidad de red de Shell de red (netsh) para administrar las tecnologías de red en Windows Server 2016 y Windows 10.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/network-subsystem/net-sub-performance-top.md">Ajuste del rendimiento del subsistema de red</a></h3>
                        <hr />
                        <p>En este tema se proporciona información acerca de cómo elegir el adaptador de red adecuado para la carga de trabajo del servidor, las interfaces de red de pedidos, los contadores de rendimiento relacionados con la red y los adaptadores de red de ajuste del rendimiento y las tecnologías de red relacionadas, como Ajuste de escala en lado de recepción (RSS), combinación de recepción (RSC) y otros.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/nic-teaming/NIC-Teaming.md">Formación de equipos NIC</a></h3>
                        <hr />
                        <p>La formación de equipos NIC permite agrupar adaptadores de red Ethernet físicos en uno o más adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/qos/qos-policy-top.md">Directiva de calidad de servicio (QoS)</a></h3>
                        <hr />
                        <p>Puedes usar la directiva QoS como un punto central de administración de ancho de banda de red en toda la infraestructura de Active Directory mediante la creación de perfiles de QoS, cuya configuración se distribuye con la directiva de grupo.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/wins/wins-top.md">Servicio de nombres Internet de Windows (WINS)</a></h3>
                        <hr />
                        <p>El servicio WINS es un servicio heredado de registro y resolución de nombres de ordenadores que asigna nombres NetBIOS de ordenador a direcciones IP. Se recomienda el uso de DNS frente al uso de WINS.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/remote-access.md">Acceso remoto</a></h3>
                        <hr />
                        <p>Puede usar tecnologías de acceso remoto, como DirectAccess y redes privadas virtuales (VPN) para proporcionar a los trabajadores remotos conectividad a los recursos de la red interna. Además, puede usar el acceso remoto para el enrutamiento de red de área local (LAN) y para el proxy de aplicación Web. que proporciona funcionalidad de proxy inversa para aplicaciones web dentro de la red corporativa, para permitir a los usuarios de cualquier dispositivo tener acceso a ellas desde fuera de la red corporativa.</p>
                        <p>Para obtener más información sobre el proxy de aplicación Web, que es un servicio de rol del rol de servidor de acceso remoto, vea <a href="https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server">proxy de aplicación web en Windows server 2016</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture">Red de contenedores de Windows</a></h3>
                        <hr />
                        <p>Redes de contenedores de Windows te permite crear y administrar redes para conectar terminales de contenedores en hosts de Windows 10 y Windows Server, usando herramientas y flujos de trabajo estándares del sector. Las redes de contenedores de Windows admiten varias topologías, incluyendo la privada, la plana L2 y la enrutada L3.</p>
                        <p>También se admiten las superposiciones que se pueden crear localmente en el host mediante Docker, Kubernetes o Windows PowerShell a través de complementos que se comunican con el servicio de red de host de Windows (SNP). Puede crear y administrar redes de clúster de varios nodos a través de sistemas de orquestación de nivel superior mediante la comunicación a través de un agente local con el SNP de cada nodo.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/vpn/vpn-top.md">Redes privadas virtuales (VPN)</a></h3>
                        <hr />
                        <p>DirectAccess y VPN es un servicio de rol del rol de servidor de acceso remoto.</p>
                        <p>Al instalar el acceso remoto como un servidor VPN, puede usar redes privadas virtuales (VPN) para proporcionar a los empleados remotos conexiones a la red de la organización a través de Internet, al tiempo que mantiene la privacidad de la información con conexiones cifradas. .</p>
                        <p> Con los equipos cliente con Windows 10 y VPN de acceso remoto de servidor de Windows, puedes implementar VPN de Always On. La VPN siempre activada te ofrece la posibilidad de administrar clientes de VPN remotos que siempre están conectados, proporcionando también a la vez comodidad para los trabajadores remotos, que ya no necesitan conectarse a la VPN de la red de la organización y desconectarse de ella.</p>
                        <p>Para obtener más información, consulte el <a href="https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy">acceso remoto Always on guía de implementación de VPN para Windows Server 2016 y Windows 10</a> .</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="additional-resources"></a>Recursos adicionales

Los recursos de redes para sistemas operativos anteriores a Windows Server 2016 se encuentran disponibles en las siguientes ubicaciones.

- [Introducción a las redes](https://technet.microsoft.com/library/hh831357.aspx) de Windows Server 2012 y Windows Server 2012 R2
- [Redes](https://technet.microsoft.com/library/cc753940) de Windows Server 2008 y Windows Server 2008 R2
- Contenido retirado de Windows Server 2003 [Windows server 2003/2003 R2](https://www.microsoft.com/download/details.aspx?id=53314)
