---
title: Novedades de redes
description: Este tema proporciona información general sobre las nuevas características y tecnologías de redes en Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43ce6290f6559be7cb078032b79519d1681506d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829196"
---
# <a name="whats-new-in-networking"></a>Novedades de redes

>Se aplica a: Windows Server 2016

Los siguientes son las tecnologías nuevas o mejoradas de redes en Windows Server 2016.  
  UPD en este tema contiene las siguientes secciones.  
  
-   [Nuevas características de red y tecnologías](#bkmk_features)  
  
-   [Nuevas características para tecnologías de redes adicionales](#bkmk_existing)  
  
## <a name="bkmk_features"></a>Nuevas características de red y tecnologías

Las redes son una parte fundamental de la plataforma del centro de datos definido por Software (SDDC) y Windows Server 2016 ofrece nuevas y mejoradas tecnologías de redes definidas por Software (SDN) para ayudarle a pasar a una solución SDDC producida por completo para su organización.  
  
Al administrar redes como un recurso definido por software, puede describir los requisitos de infraestructura de la aplicación una vez y, a continuación, elija donde se ejecuta la aplicación: localmente o en la nube.  Esta coherencia significa que las aplicaciones son ahora más fáciles de escalar y puede ejecutar sin problemas aplicaciones, en cualquier lugar, con una confianza igual en torno a seguridad, rendimiento, la calidad de servicio y la disponibilidad.  
  
Las secciones siguientes contienen información acerca de estas nuevas características y tecnologías de red.  
  
### <a name="software-defined-networking-infrastructure"></a>Infraestructura de redes definidas por software

Los siguientes son las tecnologías de infraestructura SDN nuevas o mejoradas.  
  
-   **Controladora de red**. Nuevo en Windows Server 2016, controladora de red ofrece un punto centralizado programable de la automatización para administrar, configurar, supervisar y solucionar problemas de infraestructura de red virtual y física del centro de datos. Con Controladora de red puede automatizar la configuración de la infraestructura de red, en vez de tener que configurar de forma manual los dispositivos y servicios. Para obtener más información, consulte [controladora de red](sdn/technologies/network-controller/Network-Controller.md) y [implementar Software define redes mediante secuencias de comandos](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Conmutador Virtual de Hyper-V**. El conmutador Virtual de Hyper-V se ejecuta en hosts de Hyper-V y le permite crear distribuida conmutación y enrutamiento y una capa de aplicación de directiva que es compatible con Microsoft Azure y no alineados. Para obtener más información, consulte [Conmutador virtual de Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **Virtualización de función (NFV) de red**. En software de hoy en día centros de datos definidos, las funciones de red que se realizan mediante dispositivos de hardware (por ejemplo, equilibradores de carga, firewalls, enrutadores, conmutadores y así sucesivamente) cada vez más se va a implementar como aplicaciones virtuales. Esta “virtualización de las funciones de red” es la progresión natural de la virtualización de los servidores y de la red. Dispositivos virtuales rápidamente se emergentes y creación de un mercado completamente nuevo. Se seguirán generando interés y obtener momentum en ambas plataformas de virtualización y servicios en la nube. Las siguientes tecnologías NFV están disponibles en Windows Server 2016.  
  
    -   **Datacenter Firewall**. Este firewall distribuido proporciona listas de control de acceso granular (ACL), lo que le permite aplicar directivas de firewall en el nivel de interfaz de la máquina virtual o en el nivel de subred.  
  
        Para obtener más información, consulte [información general de Datacenter Firewall](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
    -   **Puerta de enlace RAS**. Puede usar la puerta de enlace RAS para enrutar el tráfico entre redes virtuales y redes físicas, incluidas las conexiones VPN de sitio a sitio del centro de datos en la nube para sitios remotos de los inquilinos. En concreto, puede implementar el intercambio de claves por red versión 2 (IKEv2) sitio a sitio redes privadas virtuales (VPN), VPN de nivel 3 (L3) y las puertas de enlace de encapsulación de enrutamiento genérico (GRE). Además, ahora se admiten grupos de puerta de enlace y redundancia M+N de puertas de enlace; y Border Gateway Protocol (BGP) con capacidades de Reflector de ruta proporciona enrutamiento dinámico entre las redes para todos los escenarios de puerta de enlace (IKEv2 VPN, VPN GRE y L3 VPN).  
  
        Para obtener más información, consulte [Novedades de la puerta de enlace RAS](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [puerta de enlace RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Equilibrador de carga de software (SLB) y direcciones de red NAT (traducción)**. El norte sur y este-oeste de capa 4 equilibrador de carga y NAT mejora el rendimiento al admitir Direct Server Return con el que el tráfico de red de retorno puede omitir el equilibrio de carga de multiplexor.  
       Para obtener más información, consulte [equilibrio de carga de Software &#40;SLB&#41; para SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Para obtener más información, consulte [virtualización de red de función](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Estandarizado protocolos**. Controladora de red usa Representational State Transfer (REST) en la interfaz de northbound con cargas útiles de JavaScript Object Notation (JSON). La interfaz de southbound de controladora de red utiliza vSwitch abierto protocolo de administración de base de datos (OVSDB).  
  
-   **Tecnologías de encapsulación flexible**. Estas tecnologías funcionan en el plano de datos y compatibilidad con LAN extensibles virtuales (VxLAN) y Network Virtualization Generic Routing Encapsulation (NVGRE). Para obtener más información, consulte [túneles GRE en Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Para obtener más información acerca de SDN, consulte [redes definidas por Software &#40;SDN&#41;](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Fundamentos de la escala de nube
 
Los fundamentos siguientes de escala en la nube ahora están disponibles.  
  
-   **Convergente de tarjeta de interfaz de red (NIC)**. La NIC convergente le permite usar un adaptador de red para la administración de almacenamiento directo a memoria remota acceso RDMA habilitado y el tráfico de inquilinos. Esto reduce los gastos de capital que están asociados con cada servidor del centro de datos, porque necesita menos adaptadores de red para administrar distintos tipos de tráfico por servidor.  
  
-   **Packet Direct**.  Packet Direct proporciona una infraestructura de procesamiento de paquetes de baja latencia y el rendimiento del tráfico de red elevado.  
  
-   **Switch Embedded Teaming (SET)**.        CONJUNTO es una solución de formación de equipos NIC que está integrada en el conmutador Virtual de Hyper-V. CONJUNTO permite la formación de equipos NIC físicas hasta ocho en un único equipo de conjunto, lo que mejora la disponibilidad y proporciona conmutación por error. En Windows Server 2016, puede crear el conjunto de equipos que están limitados al uso de bloque de mensajes del servidor (SMB) y RDMA. Además, puede usar los equipos de conjunto para distribuir el tráfico de red para la virtualización de red de Hyper-V. Para obtener más información, consulte [acceso a memoria directa remota &#40;RDMA&#41; y Switch Embedded Teaming &#40;establecer&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>Nuevas características para tecnologías de redes adicionales

Esta sección contiene información sobre las nuevas características para tecnologías de red familiares.
  
## <a name="bkmk_dhcp"></a>DHCP  
DHCP es una norma del Grupo de trabajo en ingeniería de Internet (IETF) diseñada para reducir la carga administrativa y la complejidad de configurar hosts en una red TCP/IP, como una intranet privada. Al usar el servicio del servidor DHCP, el proceso de configuración de TCP/IP en clientes DHCP es automático.  
  
Para obtener más información, consulte [Novedades en DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="bkmk_dns"></a>DNS  
DNS es un sistema que se usa en redes TCP/IP para nombrar equipos y servicios de red. Los nombres de DNS ubican equipos y servicios a través de nombres descriptivos. Cuando un usuario escriba un nombre DNS en una aplicación, los servicios DNS pueden traducir el nombre a otra información que está asociada a dicho nombre, como una dirección IP.  
  
La siguiente es información sobre el cliente DNS y servidor DNS.  
  
### <a name="bkmk_dnsc"></a>Cliente DNS  
Los siguientes son las tecnologías de cliente DNS nuevas o mejoradas.  
  
-   **Enlace de servicio cliente DNS**. En Windows 10, el servicio cliente DNS ofrece compatibilidad mejorada para equipos con más de una interfaz de red.  
  
Para obtener más información, consulte [Novedades en el cliente DNS en Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>Servidor DNS  
Los siguientes son las tecnologías de servidor DNS nuevas o mejoradas.  
  
-   **Las directivas DNS**.  Puede configurar directivas DNS para especificar cómo un servidor DNS responde a las consultas DNS. Las respuestas DNS pueden basarse en la dirección IP del cliente (ubicación), hora del día y otros parámetros. Las directivas DNS permiten DNS con reconocimiento de ubicación, administración del tráfico, equilibrio de carga, DNS de cerebro dividido y otros escenarios.  
  
-   **Compatibilidad de nano Server para archivo basada en DNS**, puede implementar servidor DNS en Windows Server 2016 en una imagen de Nano Server. Esta opción de implementación está disponible para usted si usa DNS basados en archivos. Está ejecutando el servidor DNS en una imagen de Nano Server, puede ejecutar los servidores DNS con la superficie reducida de inicio rápido y revisiones minimizada.  
  
    > [!NOTE]   
    > DNS integrado en Active Directory no se admite en Nano Server.  
  
-   **(RRL) limitación de velocidad de respuesta**.  Puede habilitar la limitación de velocidad de respuesta en los servidores DNS. De esta manera, evitar la posibilidad de sistemas y malintencionados mediante los servidores DNS para iniciar un ataque de denegación de servicio en un cliente DNS.  
  
-   **Autenticación basada en DNS de entidades con nombre (panel)**.   Puede usar los registros TLSA (autenticación de seguridad de capa de transporte) para proporcionar información a los clientes DNS que se indican qué entidad de certificación (CA) debe esperar un certificado de nombre de dominio. Esto evita los ataques de man-in-the-middle donde alguien podría dañar la memoria caché DNS para que apunte a su propio sitio Web y proporcionar un certificado que emitieron por una CA diferente.  
  
-   **Compatibilidad con el registro desconocido**.   
     Puede agregar los registros que no están admitidos explícitamente por el servidor DNS de Windows mediante la funcionalidad de registro desconocido.  
  
-   **Las sugerencias de raíz de IPv6**.   
     Puede usar IPV6 nativo admiten sugerencias de raíz para realizar la resolución de nombres de internet mediante los servidores raíz de IPV6.  
  
-   **Compatibilidad de Windows PowerShell mejorada**.   
      Nuevos cmdlets de Windows PowerShell están disponibles para el servidor DNS.  
  
Para obtener más información, consulte [Novedades en el servidor DNS en Windows Server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>Tunelización de GRE  
Puerta de enlace RAS ahora admite túneles de encapsulación de enrutamiento genérico (GRE) de alta disponibilidad para las conexiones de sitio a sitio y redundancia M+N de puertas de enlace. GRE es un protocolo de túnel ligero que puede encapsular una amplia variedad de protocolos de capa de red dentro de los vínculos de punto a punto virtuales en una conexión entre redes de protocolo de Internet.  
  
Para obtener más información, consulte [túneles GRE en Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="HNV"></a>Virtualización de red de Hyper-V  
Se introdujo en Windows Server 2012, virtualización de red de Hyper-V (HNV) permite la virtualización de redes de cliente en una infraestructura de red física compartida. Con unos cambios mínimos necesarios en el tejido de red físico, HNV ofrece proveedores de servicios de la agilidad necesaria para implementar y migrar las cargas de trabajo de inquilino en cualquier lugar entre las tres nubes: la nube del proveedor de servicio, la nube privada o la nube pública de Microsoft Azure.  
  
Para obtener más información, consulte [Novedades de virtualización de red de Hyper-V en Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM proporciona funcionalidades de supervisión y administrativas altamente personalizables para la dirección IP y la infraestructura de DNS en una red de la organización. Con IPAM, puede supervisar, auditar y administrar servidores que ejecutan el protocolo de configuración dinámica de Host (DHCP) y sistema de nombres de dominio (DNS).  
  
-   **Una mejor administración de direcciones IP**.  
     Se han mejorado las capacidades IPAM para escenarios como control subredes /32 IPv4 y IPv6 /128 y buscar libres subredes de direcciones IP e intervalos en un bloque de direcciones IP.  
  
-   **Una mejor administración del servicio DNS**.  
     IPAM admite el registro de recursos DNS, el reenviador condicional y la administración de zonas DNS para los servidores DNS unido al dominio, integrado en Active Directory y basado en archivos.  
  
-   **Administración (DDI) de la dirección IP, DHCP y DNS integrada**.  
     Varias nuevas experiencias y están habilitadas las operaciones de administración integrada del ciclo de vida, como la visualización de todos los recursos DNS los registros que pertenecen a una dirección IP dirección, inventario automatizada de direcciones IP en función de los registros de recursos DNS y la administración del ciclo de vida de direcciones IP para las operaciones de DNS y DHCP.  
  
-   **La compatibilidad con múltiples bosques de Active Directory**.  
     Puede usar IPAM para administrar los servidores DNS y DHCP de varios bosques de Active Directory cuando hay una relación de confianza bidireccional entre el bosque donde se instala IPAM y cada uno de los bosques remotos.  
  
-   **Soporte técnico de Windows PowerShell para Control de acceso basado en rol**.  
     Puede usar Windows PowerShell para establecer ámbitos de acceso en objetos IPAM.  
  
Para obtener más información, consulte [Novedades en IPAM](technologies/ipam/What-s-New-in-IPAM.md) y [administrar IPAM](technologies/ipam/Manage-IPAM.md).  
  

