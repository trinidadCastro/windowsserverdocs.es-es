---
title: Escenarios de redes admitidos por Windows Server
description: En este tema se proporciona información sobre los nuevos escenarios de redes compatibles en Windows Server 2016 y versiones posteriores
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85f73f1f7caf833d23d3d693c0d754f52c4aa27d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812236"
---
# <a name="windows-server-supported-networking-scenarios"></a>Escenarios de redes admitidos por Windows Server

>Se aplica a: Windows Server \(canal semianual\), Windows Server 2016

En este tema se proporciona información sobre los escenarios admitidos y que puede o no se puede realizar con esta versión de Windows Server 2016.  
>[!IMPORTANT]
>Todos los escenarios de producción, use los controladores de hardware firmado más reciente de su fabricante de equipos originales \(OEM\) o proveedor de hardware independiente \(IHV\).
  
## <a name="bkmk_supp"></a>Admite escenarios de redes

Esta sección incluye información acerca de los escenarios de red admitidos para Windows Server 2016 e incluye las siguientes categorías de escenario.  
  
-   [Escenarios de software Defined Networking (SDN)](#bkmk_sdn)  
  
-   [Escenarios de la plataforma de red](#bkmk_netp)  
  
-   [Escenarios de servidor DNS](#bkmk_dns)  
  
-   [Escenarios IPAM con DHCP y DNS](#bkmk_ipam)  
  
-   [Escenarios de formación de equipos NIC](#bkmk_nicteam)

- [Switch Embedded Teaming \(establecer\) escenarios](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Escenarios de software Defined Networking (SDN)
 
Puede usar la siguiente documentación para implementar escenarios SDN con Windows Server 2016.  
  
  
-   [Implementar una infraestructura de red definida por Software con scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Para obtener más información, consulte [redes definidas por Software &#40;SDN&#41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Escenarios de controlador de red

Los escenarios de controladora de red le permiten:  
  
-   Implementar y administrar una instancia de varios nodos de controladora de red. Para obtener más información, consulte [implementar controladora de red mediante Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Usar la controladora de red para definir directivas de redes mediante programación utilizando la API Northbound de REST.  
  
-   Usar la controladora de red para crear y administrar redes virtuales con virtualización de red de Hyper-V - con encapsulación de NVGRE o VXLAN.  
  
Para obtener más información, consulte [Controladora de red](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Escenarios de virtualización de función (NFV) de red  
Los escenarios NFV le permiten:  
  
-   Implementar y usar un equilibrador de carga de software para distribuir el tráfico de northbound y southbound.  
  
-   Implementar y usar un equilibrador de carga de software para distribuir el tráfico para redes virtuales creadas con virtualización de red de Hyper-V eastbound y westbound.  
  
-   Implementar y usar un equilibrador de carga de software NAT para redes virtuales creadas con virtualización de red de Hyper-V.  
  
-   Implementar y usar una puerta de enlace de reenvío de capa 3  
  
-   Implementar y usar una puerta de enlace de red privada virtual (VPN) de túneles de IPsec (IKEv2) de sitio a sitio  
  
-   Implementar y usar una puerta de enlace de encapsulación de enrutamiento genérico (GRE).  
  
-   Implementar y configurar el enrutamiento dinámico y enrutamiento de tránsito entre sitios mediante el protocolo de puerta de enlace de borde (BGP).  
  
-   Configurar redundancia M+N Layer 3 y las puertas de enlace de sitio a sitio y para el enrutamiento de BGP.  
  
-   Usar la controladora de red para especificar las ACL en las redes virtuales y las interfaces de red.  
  
Para obtener más información, consulte [virtualización de red de función](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Escenarios de la plataforma de red

Para los escenarios de esta sección de la red de Windows Server equipo admite el uso de cualquier controlador certificados de Windows Server 2016. Por favor, póngase en contacto con su tarjeta de interfaz de red \(NIC\) fabricante para asegurarse de que las actualizaciones más recientes del controlador.
  
Los escenarios de la plataforma de red le permiten:  
  
-   Usar una NIC convergente para combinar el tráfico RDMA y Ethernet con un único adaptador de red.  
  
-   Crear una ruta de acceso de datos con latencia baja mediante Packet Direct, que está habilitada en el conmutador Virtual de Hyper-V y un adaptador de red.  
  
-   Configurar conjunto para distribuir los flujos de tráfico SMB directo y RDMA entre hasta dos adaptadores de red.  
  
Para obtener más información, consulte [acceso a memoria directa remota &#40;RDMA&#41; y Switch Embedded Teaming &#40;establecer&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Escenarios de conmutador Virtual de Hyper-V

Los escenarios de conmutador Virtual de Hyper-V le permiten:  
  
-   Crear un conmutador Virtual de Hyper-V con un vNIC de remoto acceso directo a memoria (RDMA)  
  
-   Crear un conmutador Virtual de Hyper-V con VNIC RDMA y Switch Embedded Teaming (SET)  
  
-   Crear un conjunto de equipo en el conmutador Virtual de Hyper-V  
  
-   Administrar un conjunto de equipo mediante el uso de comandos de Windows PowerShell  
  
Para obtener más información, consulte [acceso a memoria directa remota &#40;RDMA&#41; y Switch Embedded Teaming &#40;establecido&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Escenarios de servidor DNS

Escenarios de servidor DNS le permiten:  
  
-   Especifique la que ubicación geográfica en función de administración del tráfico mediante directivas de DNS  
  
-   Configurar mediante directivas de DNS de DNS de cerebro dividido  
  
-   Aplicar filtros en las consultas DNS con las directivas DNS  
  
-   Configurar el equilibrio de carga de aplicación mediante las directivas DNS  
  
-   Especificar que respuestas DNS inteligentes basadas en la hora del día  
  
-   Configurar directivas de transferencia de zona DNS  
  
-   Configurar zonas DNS server en servicios de dominio de Active Directory (AD DS) las directivas integradas  
  
-   Configurar la tasa de respuesta limitando  
  
-   Especificar la autenticación basada en DNS de entidades con nombre (panel)  
  
-   Configurar la compatibilidad con registros desconocidos en DNS  
  
Para obtener más información, vea los temas [Novedades en el cliente DNS en Windows Server 2016](dns/What-s-New-in-DNS-Client.md) y [Novedades en el servidor DNS en Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Escenarios IPAM con DHCP y DNS

Los escenarios IPAM le permiten:  
  
-   Detectar y administrar servidores DNS y DHCP y el direccionamiento IP en varios bosques de Active Directory federados  
  
-   Usar IPAM para la administración centralizada de las propiedades DNS, incluidas las zonas y registros de recursos.  
  
-   Definir directivas de control de acceso granular basada en roles y delegar IPAM o grupos de usuarios para administrar el conjunto de propiedades DNS que especifique.  
  
-   Use los comandos de Windows PowerShell para IPAM para automatizar la configuración de control de acceso para DHCP y DNS.  
  
    Para obtener más información, consulte [administrar IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Escenarios de formación de equipos NIC

Los escenarios de formación de equipos NIC le permiten:  
  
-   Creación de un equipo NIC en una configuración admitida  
  
-   Eliminar un equipo NIC  
  
-   Agregar adaptadores de red al equipo NIC en una configuración admitida  
  
-   Quite los adaptadores de red desde el equipo NIC  
  
> [!NOTE]  
> En Windows Server 2016, puede usar la formación de equipos NIC en Hyper-V, sin embargo, en algunos casos las colas de máquina Virtual (VMQ) podría no habilitar automáticamente en los adaptadores de red subyacente cuando se crea un equipo NIC. Si esto ocurre, puede usar el siguiente comando de Windows PowerShell para asegurarse de que VMQ está habilitada en los adaptadores de miembros del equipo NIC: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Para obtener más información, consulte [formación de equipos NIC](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Switch Embedded Teaming \(establecer\) escenarios

CONJUNTO es una solución alternativa de formación de equipos NIC que puede usar en entornos que incluyen Hyper-V y la pila de redes definidas por Software (SDN) en Windows Server 2016. CONJUNTO integra alguna funcionalidad de formación de equipos NIC en el conmutador Virtual de Hyper-V. 

Para obtener más información, consulte [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Escenarios de redes no admitido  
No se admiten los siguientes escenarios de redes en Windows Server 2016.  
  
-   Redes virtuales de inquilinos basada en VLAN.  
  
-   No se admite IPv6 en subposición o superposición.  
  


