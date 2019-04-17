---
title: Novedades en las redes
description: Este tema proporciona información general sobre las nuevas características y tecnologías para redes en Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85b1a573e8ac724a2a9e22863f890db668423cad
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-networking"></a>Novedades en las redes

>Se aplica a: Windows Server 2016

Los siguientes son las tecnologías de redes nuevas o mejoradas de Windows Server 2016.  
  
Este tema contiene las siguientes secciones.  
  
-   [Nuevas tecnologías y características de redes](#bkmk_features)  
  
-   [Nuevas características para otras tecnologías de redes](#bkmk_existing)  
  
## <a name="bkmk_features"></a>Nuevas tecnologías y características de redes

Funciones de red son una parte fundamental de la plataforma de Software definido Datacenter (SDDC) y Windows Server 2016 proporciona nuevas y mejoradas tecnologías de redes de definido de Software (SDN) para ayudarte a mover a una solución SDDC completamente realizada para la organización.  
  
Al administrar redes como un recurso definido de software, puedes describir los requisitos de infraestructura de la aplicación una vez y, a continuación, elige dónde la aplicación se ejecuta - en instalaciones o en la nube.  Esta coherencia significa que las aplicaciones ahora son más fáciles de escala y sin problemas se pueden ejecutar aplicaciones, desde cualquier lugar y con confianza igual alrededor de seguridad, rendimiento, calidad de servicio y disponibilidad.  
  
Las secciones siguientes contienen información acerca de estas nuevas características y tecnologías de red.  
  
### <a name="software-defined-networking-infrastructure"></a>Infraestructura de red definida de software

Los siguientes son las tecnologías de infraestructura SDN nuevas o mejoradas.  
  
-   **Controlador de red**. Novedad en Windows Server 2016, controlador de red proporciona un punto centralizado, programable de la automatización para administrar, configurar, supervisar y solucionar problemas de infraestructura de red físicas y virtuales en el centro de datos. Usar el controlador de red, puede automatizar la configuración de la infraestructura de red en lugar de realizar la configuración manual de dispositivos de red y servicios. Para obtener más información, consulta [controlador de red](sdn/technologies/network-controller/Network-Controller.md) y [implementar Software definido redes mediante scripts](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Conmutador Virtual de Hyper-V**. El conmutador Virtual Hyper-V se ejecuta en los hosts de Hyper-V y te permite crear distribuido de conmutación y enrutamiento y una capa de la aplicación de la directiva que está alineada y es compatible con Microsoft Azure. Para obtener más información, consulta [conmutador Virtual de Hyper-V ](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **Virtualización de función (NFV) de la red**. En el software de hoy centros de datos definidos, funciones de red que se realiza en dispositivos de hardware (como equilibradores de carga, firewalls, enrutadores, conmutadores y así sucesivamente) cada vez más se implementan como dispositivos virtuales. Este "virtualización en función de red" es un progreso natural de la virtualización de servidor y la virtualización de la red. Dispositivos virtuales rápidamente son emergentes y creación de un mercado totalmente nuevo. Se seguirán generar interés y obtener impulso en ambas plataformas de virtualización y servicios en la nube. Las siguientes tecnologías NFV están disponibles en Windows Server 2016.  
  
    -   **Datacenter Firewall**. Este firewall distribuida ofrece listas de control de acceso granular (ACL), lo que permite aplicar directivas de firewall en el nivel de la interfaz de máquina virtual o en el nivel de subred.  
  
        Para obtener más información, consulta [Datacenter Firewall Introducción](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
    -   **Puerta de enlace RAS**. Puedes usar la puerta de enlace RAS para enrutar el tráfico entre redes virtuales y redes físicas, incluidas las conexiones de VPN de sitio a sitio desde su centro de datos de la nube a los sitios remotos de tu los de inquilinos. Específicamente, puedes implementar el intercambio de claves de Internet versión 2 (IKEv2) sitio a sitio redes privadas virtuales (VPN), nivel 3 (L3) VPN y puertas de enlace de encapsulación de enrutamiento genérico (GRE). Además, ahora se admiten grupos de puerta de enlace y redundancia M + N puertas de enlace; y protocolo de puerta de enlace de borde (BGP) con capacidades de espejo ruta proporciona enrutamiento dinámico entre redes para todos los escenarios de puerta de enlace (IKEv2 VPN, GRE VPN y L3 VPN).  
  
        Para obtener más información, consulta [Novedades de puerta de enlace de RAS](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [RAS puerta de enlace SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Equilibrado de carga de software (SLB) y de red (NAT) de la traducción de direcciones**. El norte sur y Oriental oeste de nivel 4 equilibrado de carga y NAT mejora el rendimiento al admitir directa servidor devolver, con la que puede omitir el tráfico de red devuelto el equilibrio de carga multiplexor.  
       Para obtener más información, consulta [equilibrio de carga de Software & #40; SLB & #41; para SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Para obtener más información, consulta [virtualización de la función de red](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Estandarizado protocolos**. Controlador de red usa Representational State Transfer (REST) de su interfaz northbound con cargas de notación de objetos JavaScript (JSON). La interfaz de controlador de red southbound usa vSwitch abrir protocolo de administración de base de datos (OVSDB).  
  
-   **Tecnologías de encapsulación flexible**. Estas tecnologías funcionan en el plano de datos y admitan LAN Extensible Virtual (VxLAN) y red virtualización genérico enrutamiento encapsulación (NVGRE). Para obtener más información, consulta [GRE túnel en Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Para obtener más información sobre SDN, consulta [definido redes Software & #40; SDN & #41; ](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Conceptos básicos de escala de nube
 
Los fundamentos de escala de nube siguiente ahora están disponibles.  
  
-   **Convergido tarjeta de interfaz de red (NIC)**. La NIC convergente te permite usar un adaptador de red para la administración de almacenamiento remoto RDMA de acceso directo de memoria habilitados y el tráfico de inquilinos. Esto reduce los gastos de capital que están asociados a cada servidor en el centro de datos, porque se necesitan menos adaptadores de red para administrar los distintos tipos de tráfico por servidor.  
  
-   **Paquete Direct**.  Direct de paquete proporciona una infraestructura de procesamiento de paquetes de latencia baja y el volumen de tráfico de red alta.  
  
-   **Modificador incrustado agrupación (conjunto)**.        CONJUNTO es una solución de equipo NIC que está integrada en el conmutador de Hyper-V Virtual. Permite la agrupación de hasta ocho NIC físicas en un solo equipo conjunto, lo que mejora la disponibilidad y proporciona la conmutación por error. En Windows Server 2016, puedes crear conjunto los equipos que están restringidos para el uso de bloque de mensajes de servidor (SMB) y RDMA. Además, puedes usar el conjunto de equipos para distribuir el tráfico de red para la virtualización de Hyper-V red. Para obtener más información, consulta [remoto la acceso directo a memoria & #40; RDMA & #41; y cambiar la agrupación incrustado & #40; Establecer & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>Nuevas características para otras tecnologías de redes

Esta sección contiene información sobre las nuevas características para tecnologías de redes conocidas.
  
## <a name="bkmk_dhcp"></a>DHCP  
DHCP es un estándar de ingeniería de Internet (IETF) que está diseñado para reducir la carga administrativa y la complejidad de la configuración de hosts en una red basada en TCP/IP, como una intranet privada. Mediante el servicio de servidor DHCP, el proceso de configuración de TCP/IP en clientes DHCP es automático.  
  
Para obtener más información, consulta [Novedades de DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="bkmk_dns"></a>DNS  
DNS es un sistema que se usa en las redes TCP/IP para asignar nombre a equipos y servicios de red. Asignación de nombres DNS busca equipos y servicios con nombres descriptivos. Cuando un usuario escribe un nombre DNS de una aplicación, los servicios DNS pueden resolver el nombre con otra información que está asociado con el nombre, como una dirección IP.  
  
La siguiente es la información acerca de DNS cliente y servidor DNS.  
  
### <a name="bkmk_dnsc"></a>Cliente DNS  
Los siguientes son las tecnologías de cliente DNS nuevas o mejoradas.  
  
-   **Enlace de servicio del cliente DNS**. En Windows 10, el servicio cliente DNS ofrece compatibilidad mejorada para equipos con más de una interfaz de red.  
  
Para obtener más información, consulta [Novedades en el cliente DNS en Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>Servidor DNS  
Los siguientes son las tecnologías de servidor DNS nuevas o mejoradas.  
  
-   **Las directivas DNS**.  Puedes configurar las directivas DNS para especificar cómo un servidor DNS responde a las consultas DNS. Las respuestas de DNS pueden basarse en la dirección IP del cliente (ubicación), hora del día y otros parámetros. Las directivas DNS permiten DNS con reconocimiento de ubicación, administración del tráfico, equilibrio de carga, produzca DNS y otros escenarios.  
  
-   **Compatibilidad de nano servidor de archivos en función de DNS**, puedes implementar servidor DNS en Windows Server 2016 en una imagen de servidor Nano. Esta opción de implementación está disponible si estás usando DNS basado en archivos. Ejecuta el servidor DNS en una imagen de servidor Nano, puede ejecutar los servidores DNS con reducida la superficie, rápido de arranque y la aplicación de revisión minimizado.  
  
    > [!NOTE]   
    > DNS integrado en Active Directory no se admite en el servidor de Nano.  
  
-   **Velocidad de respuesta (RRL) limitación**.  Puedes habilitar la limitación de velocidad de respuesta en los servidores DNS. Al hacer esto, evitar la posibilidad de sistemas malintencionados con los servidores DNS para iniciar un ataque de denegación de servicio en un cliente DNS.  
  
-   **Autenticación basada en DNS de entidades con nombre (DANE)**.   Puedes usar los registros TLSA (autenticación de seguridad de capa de transporte) para proporcionar información a los clientes DNS que indica qué entidad de certificación (CA), esperan un certificado de tu nombre de dominio. Esto impide que los ataques de man-in-the-middle donde alguien podría dañar la caché para apuntar a su sitio Web lograda y proporcionar un certificado que emitieron por una CA diferentes.  
  
-   **Compatibilidad con el registro desconocido**.   
     Puedes agregar los registros que no se admiten explícitamente en el servidor DNS de Windows mediante la funcionalidad de registro desconocido.  
  
-   **Sugerencias de raíz IPv6**.   
     Puedes usar el IPV6 nativo admiten sugerencias de raíz para realizar la resolución de nombre de internet con los servidores de raíz IPV6.  
  
-   **Windows PowerShell compatibilidad mejorada**.   
      Nuevos cmdlets de PowerShell de Windows están disponibles para el servidor DNS.  
  
Para obtener más información, consulta [Novedades en el servidor DNS en Windows Server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>GRE túnel  
Puerta de enlace de RAS ahora admite túneles encapsulación de enrutamiento genérico (GRE) de alta disponibilidad para las conexiones de un sitio a otro y redundancia M + N puertas de enlace. GRE es un protocolo de túnel ligero que se puede encapsular una amplia variedad de protocolos de nivel de red dentro de los vínculos de punto a punto virtual a través de una red de protocolo de Internet.  
  
Para obtener más información, consulta [GRE túnel en Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="HNV"></a>Virtualización de la red de Hyper-V  
Introducidas en Windows Server 2012, la virtualización de red de Hyper-V (HNV) permite la virtualización de redes de cliente por encima de una infraestructura física de red compartida. Con cambios mínimos necesarios en la estructura de la red física, HNV ofrece a proveedores de servicios de la agilidad para implementar y migrar las cargas de trabajo de inquilino en cualquier lugar en las tres nubes: la nube de proveedor de servicio, la nube privada o la nube pública de Microsoft Azure.  
  
Para obtener más información, consulta [Novedades en la virtualización de la red de Hyper-V en Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM proporciona capacidades de administración y supervisión altamente personalizables para la dirección IP y la infraestructura DNS en una red de la organización. IPAM puede supervisar, auditar y administrar los servidores que ejecutan el protocolo de configuración dinámica de Host (DHCP) y el sistema de nombres de dominio (DNS).  
  
-   **Mejorada la administración de direcciones IP**.  
     Se han mejorado las funcionalidades IPAM para escenarios como control de subredes /32 IPv4 y IPv6 /128 y encontrar gratuitas subredes con direcciones IP y los intervalos en un bloque de direcciones IP.  
  
-   **Mejor administración del servicio DNS**.  
     IPAM admite el registro de recursos DNS, reenviador condicional y la administración de la zona DNS para los servidores DNS Unidos a un dominio, integrada en Active Directory y respaldados por el archivo.  
  
-   **Administración (DDI) de la dirección IP, DHCP y DNS integrada**.  
     Varias nuevas experiencias y administración del ciclo de vida integrado se habilitan las operaciones, como la visualización de todos los registros de recursos DNS que pertenecen a una dirección IP, automatizar el inventario de direcciones IP basadas en registros de recursos DNS y administración del ciclo de vida de direcciones IP para las operaciones de DNS y DHCP.  
  
-   **Compatibilidad con múltiples de bosque de Active Directory**.  
     Puedes usar IPAM para administrar los servidores DNS y DHCP de varios bosques de Active Directory cuando hay una relación de confianza bidireccional entre el bosque donde está instalado IPAM y cada uno de los bosques remotos.  
  
-   **Soporte técnico de Windows PowerShell para el Control de acceso en función de funciones**.  
     Puedes usar Windows PowerShell para establecer los ámbitos de acceso de los objetos IPAM.  
  
Para obtener más información, consulta [Novedades de IPAM](technologies/ipam/What-s-New-in-IPAM.md) y [administrar IPAM](technologies/ipam/Manage-IPAM.md).  
  

