---
title: Novedades de redes
description: En este tema se proporciona información general sobre las nuevas características y tecnologías de las funciones de red en Windows Server 2016
ms.prod: windows-server
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
author: dcuomo
ms.author: dacuo
ms.openlocfilehash: 0a3d7bb4f8521493743024408ae0f1f35a4b251c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855798"
---
# <a name="whats-new-in-networking"></a>Novedades de redes

>Se aplica a: Windows Server 2016

A continuación se enumeran las tecnologías de red nuevas o mejoradas en Windows Server 2016.  
  UPD este tema contiene las siguientes secciones.  
  
-   [Nuevas características y tecnologías de red](#bkmk_features)  
  
-   [Nuevas características para tecnologías de red adicionales](#bkmk_existing)  
  
## <a name="new-networking-features-and-technologies"></a><a name="bkmk_features"></a>Nuevas características y tecnologías de red

La red es una parte fundamental de la plataforma del centro de centros de recursos (SDDC) de software, y Windows Server 2016 proporciona tecnologías de redes definidas por software (SDN) nuevas y mejoradas que le ayudarán a migrar a una solución de SDDC completa para su organización.  
  
Al administrar redes como un recurso definido por software, puede describir los requisitos de infraestructura de una aplicación una vez y, después, elegir dónde se ejecuta la aplicación en el entorno local o en la nube.  Esta coherencia significa que las aplicaciones son ahora más fáciles de escalar y puede ejecutar sin problemas aplicaciones, en cualquier lugar, con la misma confianza en lo que se refiere a la seguridad, el rendimiento, la calidad del servicio y la disponibilidad.  
  
Las secciones siguientes contienen información acerca de estas nuevas características y tecnologías de red.  
  
### <a name="software-defined-networking-infrastructure"></a>Infraestructura de red definida por software

A continuación se indican las tecnologías de infraestructura de SDN nuevas o mejoradas.  
  
-   **Controladora de red**. Como novedad en Windows Server 2016, la controladora de red proporciona un punto de automatización programable y centralizado para administrar, configurar, supervisar y solucionar problemas de la infraestructura de red virtual y física en el centro de recursos. Con Controladora de red puede automatizar la configuración de la infraestructura de red, en vez de tener que configurar de forma manual los dispositivos y servicios. Para obtener más información, consulte [controladora de red](sdn/technologies/network-controller/Network-Controller.md) e [Implementar redes definidas por software mediante scripts](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Conmutador virtual de Hyper-V**. El conmutador virtual de Hyper-V se ejecuta en hosts de Hyper-V y permite crear conmutación y enrutamiento distribuido, y una capa de cumplimiento de directivas que se alinea y es compatible con Microsoft Azure. Para obtener más información, consulte [Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **Virtualización de función de red (NFV)** . En los centros de recursos definidos por software de hoy en día, las funciones de red que realizan los dispositivos de hardware (por ejemplo, equilibradores de carga, firewalls, enrutadores, conmutadores, etc.) se implementan cada vez más como aplicaciones virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red. Las aplicaciones virtuales están surgiendo rápidamente y crean un nuevo mercado. Continúan generando interés y ganar impulsos en las plataformas de virtualización y en los servicios en la nube. Las siguientes tecnologías de NFV están disponibles en Windows Server 2016.  
  
    -   **Firewall de Datacenter**. Este firewall distribuido proporciona listas de control de acceso (ACL) pormenorizadas, lo que le permite aplicar directivas de firewall en el nivel de interfaz de VM o en el nivel de subred.  
  
        Para obtener más información, consulte [información general sobre el firewall del centro](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)de datos.  
  
    -   **Puerta de enlace ras**. Puede usar la puerta de enlace RAS para enrutar el tráfico entre las redes virtuales y las redes físicas, incluidas las conexiones VPN de sitio a sitio desde el centro de servicios en la nube a los sitios remotos de los inquilinos. En concreto, puede implementar Intercambio de claves por red redes privadas virtuales (VPN) de sitio a sitio de la versión 2 (IKEv2), VPN de nivel 3 (L3) y de encapsulación de enrutamiento genérico (GRE). Además, ahora se admiten los grupos de puertas de enlace y la redundancia M + N de las puertas de enlace; y Protocolo de puerta de enlace de borde (BGP) con capacidades de reflector de rutas proporciona enrutamiento dinámico entre redes para todos los escenarios de puerta de enlace (VPN IKEv2, VPN GRE y VPN L3).  
  
        Para obtener más información, consulte [novedades de puerta de enlace ras](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [puerta de enlace ras para Sdn](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Load balancer de software (SLB) y traducción de direcciones de red (NAT)** . El equilibrador de carga de nivel 4 norte-sur y este-oeste de Europa mejora el rendimiento al admitir Direct Server Return, con el que el tráfico de red devuelto puede omitir el multiplexor de equilibrio de carga.  
       Para más información, consulte [ &#40;equilibrio de carga de&#41; software SLB para Sdn](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Para obtener más información, consulte [virtualización de función de red](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Protocolos estandarizados**. La controladora de red usa la transferencia de estado representacional (REST) en su interfaz Northbound con cargas notación de objetos JavaScript (JSON). La interfaz southbound de la controladora de red usa Open vSwitch Database Management Protocol (OVSDB).  
  
-   **Tecnologías de encapsulación flexibles**. Estas tecnologías operan en el plano de datos y son compatibles con la encapsulación de enrutamiento genérico de virtualización de red (NVGRE) y virtual extensible LAN (VxLAN). Para obtener más información, consulte [tunelización de GRE en Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Para obtener más información acerca de SDN, consulte [software &#40;Defined&#41;Networking Sdn](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Aspectos básicos de la escala de nube
 
Los siguientes aspectos básicos de la escala de nube ahora están disponibles.  
  
-   **Tarjeta de interfaz de red (NIC) convergente**. La NIC convergente le permite usar un único adaptador de red para la administración, el almacenamiento habilitado para el acceso directo a memoria remota (RDMA) y el tráfico de inquilinos. Esto reduce los gastos de capital asociados a cada servidor del centro de recursos, ya que se necesitan menos adaptadores de red para administrar distintos tipos de tráfico por servidor.  
  
-   **Paquete directo**.  Packet Direct proporciona un alto rendimiento del tráfico de red y una infraestructura de procesamiento de paquetes de latencia baja.  
  
-   **Cambiar la formación de equipos incrustada (establecer)** .        SET es una solución de formación de equipos NIC que se integra en el conmutador virtual de Hyper-V. SET permite la formación de equipos de hasta ocho NIC físicas en un único equipo de conjunto, lo que mejora la disponibilidad y proporciona conmutación por error. En Windows Server 2016, puede crear equipos de conjunto restringidos al uso del bloque de mensajes del servidor (SMB) y RDMA. Además, puede usar SET Teams para distribuir el tráfico de red para la virtualización de red de Hyper-V. Para obtener más información, vea [acceso directo a &#40;memoria&#41; remota RDMA y switch Embedded &#40;Teaming set&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="new-features-for-additional-networking-technologies"></a><a name="bkmk_existing"></a>Nuevas características para tecnologías de red adicionales

Esta sección contiene información acerca de las nuevas características de las tecnologías de red conocidas.
  
## <a name="dhcp"></a><a name="bkmk_dhcp"></a>DHCP  
DHCP es una norma del Grupo de trabajo en ingeniería de Internet (IETF) diseñada para reducir la carga administrativa y la complejidad de configurar hosts en una red TCP/IP, como una intranet privada. Al usar el servicio del servidor DHCP, el proceso de configuración de TCP/IP en clientes DHCP es automático.  
  
Para obtener más información, consulte [novedades de DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="dns"></a><a name="bkmk_dns"></a>DN  
DNS es un sistema que se usa en redes TCP/IP para nombrar equipos y servicios de red. Los nombres de DNS ubican equipos y servicios a través de nombres descriptivos. Cuando un usuario escriba un nombre DNS en una aplicación, los servicios DNS puden traducir el nombre a otra información que está asociada a dicho nombre, como una dirección IP.  
  
A continuación se encuentra información sobre el cliente DNS y el servidor DNS.  
  
### <a name="dns-client"></a><a name="bkmk_dnsc"></a>Cliente DNS  
A continuación se indican las tecnologías cliente DNS nuevas o mejoradas.  
  
-   **Enlace de servicio de cliente DNS**. En Windows 10, el servicio cliente DNS ofrece compatibilidad mejorada para equipos con más de una interfaz de red.  
  
Para obtener más información, vea [novedades del cliente DNS en Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="dns-server"></a><a name="bkmk_dnss"></a>Servidor DNS  
A continuación se indican las tecnologías de servidor DNS nuevas o mejoradas.  
  
-   **Directivas DNS**.  Puede configurar directivas DNS para especificar el modo en que un servidor DNS responde a las consultas DNS. Las respuestas DNS pueden basarse en la dirección IP del cliente (ubicación), la hora del día y otros parámetros. Las directivas de DNS permiten el DNS, la administración del tráfico, el equilibrio de carga, el DNS de cerebro dividido y otros escenarios que tienen en cuenta la ubicación.  
  
-   **Compatibilidad de nano Server con DNS basado en archivos**: puede implementar el servidor DNS en Windows Server 2016 en una imagen de nano Server. Esta opción de implementación está disponible si usa DNS basado en archivos. Al ejecutar el servidor DNS en una imagen de nano Server, puede ejecutar los servidores DNS con una superficie reducida, un arranque rápido y una revisión minimizada.  
  
    > [!NOTE]   
    > No se admite el DNS integrado Active Directory en nano Server.  
  
-   **Limitación de velocidad de respuesta (RRL)** .  Puede habilitar la limitación de la velocidad de respuesta en los servidores DNS. Al hacerlo, evita la posibilidad de que los sistemas malintencionados usen los servidores DNS para iniciar un ataque de denegación de servicio en un cliente DNS.  
  
-   **Autenticación basada en DNS de entidades con nombre (Sundanés)** .   Puede usar los registros TLSA (autenticación de seguridad de la capa de transporte) para proporcionar información a los clientes DNS que indiquen la entidad de certificación (CA) a la que deben esperar un certificado para su nombre de dominio. Esto evita ataques de tipo "Man in the Middle", en los que alguien podría dañar la memoria caché de DNS para apuntar a su propio sitio web y proporcionar un certificado emitido por una CA diferente.  
  
-   **Compatibilidad con registros desconocidos**.   
     Puede agregar registros que no son compatibles explícitamente con el servidor DNS de Windows mediante la funcionalidad de registros desconocidos.  
  
-   **Sugerencias de raíz IPv6**.   
     Puede usar la compatibilidad con las sugerencias de raíz IPV6 nativas para realizar la resolución de nombres de Internet mediante los servidores raíz IPV6.  
  
-   **Compatibilidad mejorada con Windows PowerShell**.   
      Hay nuevos cmdlets de Windows PowerShell disponibles para el servidor DNS.  
  
Para obtener más información, consulta [What's New in DNS Server in Windows server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="gre-tunneling"></a><a name="bkmk_GRE"></a>Tunelización de GRE  
La puerta de enlace RAS admite ahora túneles de encapsulación de enrutamiento genérico (GRE) de alta disponibilidad para las conexiones de sitio a sitio y la redundancia M + N de las puertas de enlace. GRE es un protocolo de túnel ligero que puede encapsular una amplia variedad de protocolos de capa de red dentro de los vínculos de punto a punto virtuales en una conexión entre redes de protocolo de Internet.  
  
Para obtener más información, consulte [tunelización de GRE en Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="hyper-v-network-virtualization"></a><a name="HNV"></a>Virtualización de red de Hyper-V  
La virtualización de red de Hyper-V (HNV), que se presentó en Windows Server 2012, permite la virtualización de redes de clientes sobre una infraestructura de red física compartida. Con los cambios mínimos necesarios en el tejido de red físico, HNV ofrece a los proveedores de servicios la agilidad para implementar y migrar cargas de trabajo de inquilino en cualquier parte de las tres nubes: la nube del proveedor de servicios, la nube privada o la Microsoft Azure nube pública.  
  
Para obtener más información, consulte [novedades de virtualización de red de Hyper-V en Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="ipam"></a><a name="bkmk_ipam"></a>IPAM  
IPAM proporciona capacidades de supervisión y administración muy personalizables para la dirección IP y la infraestructura DNS en una red de la organización. Con IPAM, puede supervisar, auditar y administrar servidores que ejecutan el protocolo de configuración dinámica de host (DHCP) y el sistema de nombres de dominio (DNS).  
  
-   **Administración mejorada de direcciones IP**.  
     Las funcionalidades de IPAM se han mejorado en escenarios como el control de subredes IPv4/32 y IPv6/128 y la búsqueda de subredes de direcciones IP gratuitas e intervalos en un bloque de direcciones IP.  
  
-   **Administración de servicios DNS mejorada**.  
     IPAM admite registros de recursos DNS, reenviadores condicionales y administración de zonas DNS para los servidores DNS Active Directory Unidos a un dominio y los basados en archivos.  
  
-   **Administración de DNS, DHCP y dirección IP (DDI) integrada**.  
     Se habilitan varias experiencias nuevas y las operaciones de administración del ciclo de vida integradas, como la visualización de todos los registros de recursos DNS que pertenecen a una dirección IP, el inventario automatizado de direcciones IP basado en registros de recursos DNS y la administración del ciclo de vida de las direcciones IP. tanto para las operaciones DNS como para DHCP.  
  
-   **Compatibilidad con varios Active Directory bosques**.  
     Puede usar IPAM para administrar los servidores DNS y DHCP de varios bosques de Active Directory cuando hay una relación de confianza bidireccional entre el bosque en el que está instalado IPAM y cada uno de los bosques remotos.  
  
-   **Compatibilidad de Windows PowerShell con el Access Control basado en roles**.  
     Puede usar Windows PowerShell para establecer ámbitos de acceso en objetos IPAM.  
  
Para obtener más información, vea [novedades de IPAM](technologies/ipam/What-s-New-in-IPAM.md) y [Administración de IPAM](technologies/ipam/Manage-IPAM.md).  
  

