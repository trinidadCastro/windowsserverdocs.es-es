---
title: Windows Server compatible de escenarios de red
description: Este tema proporciona información acerca de nuevos escenarios de red compatibles en Windows Server 2016 y versiones posteriores
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 70198f97c4ec39de4b78de28ab196dc3e86a684c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server compatible de escenarios de red

>Se aplica a: Windows \(Semi-Annual Channel\) Server, Windows Server 2016

Este tema proporciona información sobre los escenarios admitidos y que puede o no se puede realizar con esta versión de Windows Server 2016.  
>[!IMPORTANT]
>Para todos los escenarios de producción, usa los controladores más recientes firmado hardware desde el fabricante de equipos originales \(OEM\) o proveedor de hardware independiente \(IHV\).
  
## <a name="bkmk_supp"></a>Escenarios de red compatibles

Esta sección incluye información acerca de los escenarios de red compatibles para Windows Server 2016 e incluye las siguientes categorías de escenario.  
  
-   [Escenarios de red definido (SDN) de software](#bkmk_sdn)  
  
-   [Escenarios de la plataforma de red](#bkmk_netp)  
  
-   [Escenarios de servidor DNS](#bkmk_dns)  
  
-   [Escenarios IPAM con DHCP y DNS](#bkmk_ipam)  
  
-   [Escenarios de equipo NIC](#bkmk_nicteam)

- [Cambiar la agrupación incrustado \(SET\) escenarios](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Escenarios de red definido (SDN) de software
 
Puedes usar la siguiente documentación para implementar los escenarios de SDN con Windows Server 2016.  
  
  
-   [Implementar una infraestructura de red definido de Software mediante scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Para obtener más información, consulta [definido redes Software & #40; SDN & #41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Escenarios de controlador de red

Los escenarios de controlador de red le permiten:  
  
-   Implementar y administrar una instancia de varios nodos de controlador de red. Para obtener más información, consulta [implementar controladores de red mediante Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Usar el controlador de red para definir mediante programación la directiva de red mediante el uso de la API de REST Northbound.  
  
-   Usar el controlador de red para crear y administrar redes virtuales con la virtualización de la red de Hyper-V - con encapsulación NVGRE o VXLAN.  
  
Para obtener más información, consulta [controlador de red](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Escenarios de virtualización de la función (NFV) de red  
Los escenarios NFV permiten:  
  
-   Implementar y usar un equilibrado de carga de software para distribuir el tráfico northbound y southbound.  
  
-   Implementar y usar un equilibrado de carga de software para distribuir el tráfico eastbound y westbound de redes virtuales creadas con la virtualización de Hyper-V red.  
  
-   Implementar y usar un equilibrado de carga de software NAT de redes virtuales creadas con la virtualización de Hyper-V red.  
  
-   Implementar y usar una puerta de enlace de reenvío de capa 3  
  
-   Implementar y usar una puerta de enlace de red privada virtual (VPN) para los túneles de IPsec (IKEv2) del sitio a sitio  
  
-   Implementar y usar una puerta de enlace de encapsulación de enrutamiento genérico (GRE).  
  
-   Implementar y configurar dinámico y transporte público de enrutamiento entre sitios mediante el protocolo de puerta de enlace de borde (BGP).  
  
-   Configurar redundancia M + N de nivel 3 y puertas de enlace de sitio a sitio y para el enrutamiento de BGP.  
  
-   Usar el controlador de red para especificar las ACL en las redes virtuales e interfaces de red.  
  
Para obtener más información, consulta [virtualización de la función de red](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Escenarios de la plataforma de red

Para los escenarios de esta sección de la red de Windows Server equipo admite el uso de cualquier controlador certificados de Windows Server 2016. Ponte en contacto con el fabricante \(NIC\) de tarjeta de interfaz de red para asegurarse de que tienes las últimas actualizaciones de controladores.
  
Los escenarios de la plataforma de red le permiten:  
  
-   Usa una NIC convergente para combinar el tráfico RDMA y Ethernet mediante un adaptador de red.  
  
-   Crear una ruta de acceso de los datos de latencia baja mediante paquetes Direct, habilitado en el conmutador Virtual Hyper-V y un adaptador de red.  
  
-   Configurar conjunto repartir SMB directa y RDMA flujos de tráfico entre un máximo de dos adaptadores de red.  
  
Para obtener más información, consulta [remoto la acceso directo a memoria & #40; RDMA & #41; y cambiar la agrupación incrustado & #40; Establecer & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Escenarios de conmutador Virtual de Hyper-V

Los escenarios de Hyper-V Virtual Switch permiten:  
  
-   Crear un modificador de Hyper-V Virtual con un vNIC de memoria de acceso directo remoto (RDMA)  
  
-   Crear un conmutador Virtual de Hyper-V con el modificador incrustado agrupación (conjunto) y RDMA vNICs  
  
-   Crear un conjunto de equipo en el conmutador Virtual de Hyper-V  
  
-   Administrar un equipo conjunto mediante comandos de Windows PowerShell  
  
Para obtener más información, consulta [remoto la acceso directo a memoria & #40; RDMA & #41; y cambiar la agrupación incrustado & #40; Establecer & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Escenarios de servidor DNS

Escenarios de servidor DNS permiten:  
  
-   Especificar la que ubicación geográfica en función de administración de tráfico mediante las directivas de DNS  
  
-   Configurar produzca DNS mediante las directivas de DNS  
  
-   Aplicar filtros de consultas DNS mediante las directivas de DNS  
  
-   Configurar la aplicación mediante las directivas de DNS de equilibrio de carga  
  
-   Especificar que las respuestas de DNS inteligente en función de la hora del día  
  
-   Configurar las directivas de transferencia de zona DNS  
  
-   Configurar zonas de directivas en los servicios de dominio de Active Directory (AD DS) integradas de servidor DNS  
  
-   Configurar la velocidad de respuesta limitar  
  
-   Especificar la autenticación basada en DNS de entidades con nombre (DANE)  
  
-   Configurar la compatibilidad con registros desconocidos en DNS  
  
Para obtener más información, consulta los temas [Novedades en el cliente DNS en Windows Server 2016](dns/What-s-New-in-DNS-Client.md) y [Novedades en el servidor DNS en Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Escenarios IPAM con DHCP y DNS

Los escenarios IPAM permiten:  
  
-   Descubrir y administrar los servidores DNS y DHCP y direcciones IP en varios bosques de Active Directory federados  
  
-   Usa IPAM para una administración centralizada de propiedades DNS, incluidas las zonas y registros de recursos.  
  
-   Definir las directivas de control de acceso granular basada en rol y el delegado IPAM o grupos de usuarios para administrar el conjunto de propiedades DNS que especifiques.  
  
-   Usar los comandos de Windows PowerShell para IPAM para automatizar la configuración de control de acceso para DHCP y DNS.  
  
    Para obtener más información, consulta [administrar IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Escenarios de equipo NIC

Los escenarios de equipo NIC permiten:  
  
-   Crear un equipo NIC en una configuración compatible  
  
-   Eliminar un equipo NIC  
  
-   Agregar adaptadores de red al equipo NIC en una configuración compatible  
  
-   Quitar adaptadores de red del equipo NIC  
  
> [!NOTE]  
> En Windows Server 2016, puedes usar el equipo NIC en Hyper-V, pero en algunos casos colas de máquina Virtual (VMQ) es posible que se habilita automáticamente en los adaptadores de red subyacente cuando se crea un equipo NIC. Si esto ocurre, puedes usar el siguiente comando de Windows PowerShell para garantizar que VMQ esté habilitado en los adaptadores de miembro del equipo NIC: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Para obtener más información, consulta [equipo NIC](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Cambiar la agrupación incrustado \(SET\) escenarios

CONJUNTO es una solución alternativa equipo NIC que puedes usar en entornos que incluyen Hyper-V y la pila de Software definido de redes (SDN) en Windows Server 2016. CONJUNTO integra algunas funciones de equipo NIC en el conmutador de Hyper-V Virtual. 

Para obtener más información, consulta [memoria de acceso directo remoto (RDMA) y cambiar incrustado agrupación (conjunto)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Escenarios de red no compatibles  
No se admiten los siguientes escenarios de red en Windows Server 2016.  
  
-   Redes virtuales en función de VLAN inquilino.  
  
-   Superposición o subposición no admite IPv6.  
  


