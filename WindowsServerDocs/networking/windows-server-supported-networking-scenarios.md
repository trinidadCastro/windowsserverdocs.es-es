---
title: Escenarios de redes que admite Windows Server
description: En este tema se proporciona información sobre los nuevos escenarios de redes compatibles en Windows Server 2016 y versiones posteriores.
manager: brianlic
ms.topic: article
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
author: dcuomo
ms.author: dacuo
ms.openlocfilehash: 5a0f8d372c8e84e6e9140ef40f89c1fa7b116355
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940006"
---
# <a name="windows-server-supported-networking-scenarios"></a>Escenarios de redes que admite Windows Server

>Se aplica a: \( canal semianual de Windows Server \) , Windows Server 2016

En este tema se proporciona información acerca de los escenarios admitidos y no admitidos que puede o no puede realizar con esta versión de Windows Server 2016.
>[!IMPORTANT]
>En todos los escenarios de producción, use los controladores de hardware firmados más recientes del fabricante de equipos originales \( OEM \) o del proveedor de hardware independiente \( IHV \) .

## <a name="supported-networking-scenarios"></a><a name="bkmk_supp"></a>Escenarios de red admitidos

En esta sección se incluye información acerca de los escenarios de red admitidos para Windows Server 2016 e incluye las siguientes categorías de escenarios.

-   [Escenarios de redes definidas por software (SDN)](#bkmk_sdn)

-   [Escenarios de plataforma de red](#bkmk_netp)

-   [Escenarios de servidor DNS](#bkmk_dns)

-   [Escenarios de IPAM con DHCP y DNS](#bkmk_ipam)

-   [Escenarios de formación de equipos NIC](#bkmk_nicteam)

- [Cambiar escenarios de conjuntos de formación de equipos incrustados \( \)](#bkmk_set)

### <a name="software-defined-networking-sdn-scenarios"></a><a name="bkmk_sdn"></a>Escenarios de redes definidas por software (SDN)

Puede usar la siguiente documentación para implementar escenarios de SDN con Windows Server 2016.


-   [Implementación de una infraestructura de red definida por software con scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)

Para obtener más información, vea [redes definidas por Software &#40;SDN&#41;](sdn/software-defined-networking.md).

#### <a name="network-controller-scenarios"></a><a name="bkmk_netc"></a>Escenarios de controladora de red

Los escenarios de la controladora de red permiten:

-   Implementar y administrar una instancia de varios nodos de la controladora de red. Para obtener más información, consulte implementación de la [controladora de red con Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).

-   Use el controlador de red para definir mediante programación la Directiva de red mediante la API de REST Northbound.

-   Use el controlador de red para crear y administrar redes virtuales con la virtualización de red de Hyper-V mediante la encapsulación NVGRE o VXLAN.

Para obtener más información, consulte [Controladora de red](sdn/technologies/network-controller/Network-Controller.md).

#### <a name="network-function-virtualization-nfv-scenarios"></a><a name="bkmk_netf"></a>Escenarios de virtualización de función de red (NFV)
Los escenarios de NFV permiten:

-   Implemente y use un equilibrador de carga de software para distribuir el tráfico de Northbound y southbound.

-   Implemente y use un equilibrador de carga de software para distribuir el tráfico de Eastbound y Westbound para las redes virtuales creadas con virtualización de red de Hyper-V.

-   Implemente y use un equilibrador de carga de software NAT para redes virtuales creadas con virtualización de red de Hyper-V.

-   Implementación y uso de una puerta de enlace de reenvío de nivel 3

-   Implementación y uso de una puerta de enlace de red privada virtual (VPN) para túneles de IPsec de sitio a sitio (IKEv2)

-   Implemente y use una puerta de enlace de encapsulación de enrutamiento genérico (GRE).

-   Implementar y configurar el enrutamiento dinámico y el enrutamiento de tránsito entre sitios mediante Protocolo de puerta de enlace de borde (BGP).

-   Configure la redundancia M + N para puertas de enlace de nivel 3 y de sitio a sitio, y para el enrutamiento de BGP.

-   Use controladora de red para especificar ACL en redes virtuales e interfaces de red.

Para obtener más información, consulte [virtualización de función de red](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).

### <a name="network-platform-scenarios"></a><a name="bkmk_netp"></a>Escenarios de plataforma de red

En los escenarios de esta sección, el equipo de redes de Windows Server admite el uso de cualquier controlador certificado de Windows Server 2016. Consulte con el fabricante de la tarjeta de interfaz de red \( \) para asegurarse de que dispone de las actualizaciones más recientes del controlador.

Los escenarios de la plataforma de red permiten:

-   Use una NIC convergente para combinar el tráfico RDMA y Ethernet mediante un único adaptador de red.

-   Cree una ruta de acceso de datos de baja latencia mediante el uso de paquetes directo, habilitado en el conmutador virtual de Hyper-V y un único adaptador de red.

-   Configure SET para distribuir los flujos de tráfico de SMB directo y RDMA entre un máximo de dos adaptadores de red.

Para obtener más información, vea [acceso directo a memoria remota &#40;RDMA&#41; y switch Embedded teaming &#40;SET&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

#### <a name="hyper-v-virtual-switch-scenarios"></a><a name="bkmk_switch"></a>Escenarios del conmutador virtual de Hyper-V

Los escenarios del conmutador virtual de Hyper-V permiten:

-   Creación de un conmutador virtual de Hyper-V con acceso directo a memoria remota (RDMA) vNIC

-   Crear un conmutador virtual de Hyper-V con switch Embedded Teaming (SET) y RDMA VNIC

-   Crear un equipo de conjunto en el conmutador virtual de Hyper-V

-   Administrar un equipo de conjunto mediante comandos de Windows PowerShell

Para obtener más información, vea [acceso directo a memoria remota &#40;RDMA&#41; y switch Embedded teaming &#40;SET&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)

### <a name="dns-server-scenarios"></a><a name="bkmk_dns"></a>Escenarios de servidor DNS

Los escenarios de servidor DNS permiten:

-   Especificación de la administración del tráfico basada en la ubicación geográfica mediante directivas DNS

-   Configuración de DNS de cerebro dividido mediante directivas DNS

-   Aplicar filtros en consultas DNS mediante directivas DNS

-   Configurar el equilibrio de carga de la aplicación mediante directivas DNS

-   Especificar respuestas DNS inteligentes basadas en la hora del día

-   Configurar directivas de transferencia de zona DNS

-   Configurar directivas de servidor DNS en zonas integradas de Active Directory Domain Services (AD DS)

-   Configurar la limitación de velocidad de respuesta

-   Especificar la autenticación basada en DNS de entidades con nombre (SUNDANÉS)

-   Configuración de la compatibilidad con registros desconocidos en DNS

Para obtener más información, vea los temas [novedades del cliente DNS en Windows server 2016](dns/What-s-New-in-DNS-Client.md) y [novedades del servidor DNS en Windows Server 2016](dns/What-s-New-in-DNS-Server.md).

### <a name="ipam-scenarios-with-dhcp-and-dns"></a><a name="bkmk_ipam"></a>Escenarios de IPAM con DHCP y DNS

Los escenarios de IPAM permiten:

-   Detección y administración de servidores DNS y DHCP y direccionamiento IP en varios bosques de Active Directory federados

-   Use IPAM para la administración centralizada de las propiedades de DNS, incluidas las zonas y los registros de recursos.

-   Defina directivas de control de acceso basado en roles y delegue usuarios o grupos de usuarios de IPAM para administrar el conjunto de propiedades DNS que especifique.

-   Use los comandos de Windows PowerShell para IPAM para automatizar la configuración de control de acceso para DHCP y DNS.

    Para obtener más información, vea [administrar IPAM](technologies/ipam/Manage-IPAM.md).

### <a name="nic-teaming-scenarios"></a><a name="bkmk_nicteam"></a>Escenarios de formación de equipos NIC

Los escenarios de formación de equipos NIC permiten:

-   Crear un equipo NIC en una configuración compatible

-   Eliminación de un equipo NIC

-   Agregar adaptadores de red al equipo NIC en una configuración compatible

-   Quitar adaptadores de red del equipo NIC

> [!NOTE]
> En Windows Server 2016, puede usar la formación de equipos NIC en Hyper-V; sin embargo, en algunos casos, es posible que las colas de máquinas virtuales (VMQ) no se habiliten automáticamente en los adaptadores de red subyacentes al crear un equipo NIC. Si esto ocurre, puede usar el siguiente comando de Windows PowerShell para asegurarse de que VMQ está habilitado en los adaptadores de miembros del equipo NIC:`Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`

Para obtener más información, consulte [formación de equipos NIC](technologies/nic-teaming/NIC-Teaming.md).

### <a name="switch-embedded-teaming-set-scenarios"></a><a name="bkmk_set"></a>Cambiar escenarios de conjuntos de formación de equipos incrustados \( \)

SET es una solución alternativa para la formación de equipos NIC que puede usar en entornos que incluyen Hyper-V y la pila de redes definidas por software (SDN) en Windows Server 2016. El conjunto integra la funcionalidad de formación de equipos NIC en el conmutador virtual de Hyper-V.

Para obtener más información, vea [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming) .



## <a name="unsupported-networking-scenarios"></a><a name="bkmk_unsupp"></a>Escenarios de redes no admitidos
Los siguientes escenarios de red no se admiten en Windows Server 2016.

-   Redes virtuales de inquilino basadas en VLAN.

-   IPv6 no se admite en proporcionaban ni en la superposición.



