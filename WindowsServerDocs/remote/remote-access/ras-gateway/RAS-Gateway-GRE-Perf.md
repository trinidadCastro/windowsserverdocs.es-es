---
title: Rendimiento de túnel GRE de puerta de enlace de RAS
description: En este tema, que está pensado para profesionales de tecnología de información (TI), se proporciona información de rendimiento de rendimiento acerca de los túneles de RAS puerta de enlace de enrutamiento encapsulación genérico (GRE).
manager: brianlic
ms.prod: windows-server-threshold
ms.date: ''
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73ae4e573d926f4a77b076c880c1d74ed69f032d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880766"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>Rendimiento de túnel GRE de puerta de enlace de RAS

>Se aplica a: Windows Server \(canal semianual\)

Puede usar este tema para obtener información sobre el servidor de acceso remoto \(RAS\) puerta de enlace de encapsulación de enrutamiento genérico \(GRE\) tunelizar el rendimiento en Windows Server, versión 1709, en un no - redes definidas por Software \( SDN\) entorno de prueba basada en.

Puerta de enlace RAS es un enrutador de software y la puerta de enlace que puede usar en modo de inquilino único o en modo multiempresa. Este tema describe un modo único inquilino, la configuración de alta disponibilidad con clústeres de conmutación por error. Las estadísticas de rendimiento de túnel GRE que se presentan en este tema son válidas para la puerta de enlace de RAS en inquilinos singele y modos de varios inquilinos.

>[!NOTE]
>Agrupación en clústeres de conmutación por error es una característica de Windows Server que permite agrupar varios servidores en un clúster con tolerancia. Para obtener más información, consulte [agrupación en clústeres de conmutación por error](../../../failover-clustering/failover-clustering-overview.md)

Modo de un solo inquilino permite a las organizaciones de cualquier tamaño para implementar la puerta de enlace como un exterior o Internet\-orientado a la red privada virtual de edge \(VPN\) server. En el modo de un solo inquilino, puede implementar la puerta de enlace de RAS en un servidor físico o máquina virtual \(VM\). Este tema describe la implementación de puerta de enlace de RAS en dos máquinas virtuales \(máquinas virtuales\) que están configurados en un clúster de conmutación por error.

>[!IMPORTANT]
>Porque los túneles GRE proporcionan encapsulación, pero no cifrado, no debe usar la puerta de enlace de RAS configurada con GRE como una puerta de enlace de borde de Internet. Para obtener información acerca de los mejores usos de puerta de enlace de RAS con túneles GRE, consulte [túneles GRE en Windows Server](gre-tunneling-windows-server.md).

GRE es una ligera protocolo que puede encapsular una amplia variedad de protocolos de capa de red dentro de un punto virtual de túnel\-a\-vínculos de punto a través de un conjunto de redes de protocolo de Internet. La implementación de Microsoft GRE encapsula IPv4 e IPv6.

Para obtener más información, consulte la sección **escenarios de implementación de puerta de enlace de RAS** en el tema [puerta de enlace RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy). 

En este escenario de prueba, que se muestra en la siguiente ilustración, el flujo de tráfico se mide se mueve de la organización Intranet 2 out el 1 de Intranet de la organización. Máquinas virtuales de carga de trabajo de inquilino enviar tráfico de red de Intranet 2 en 1 de la Intranet mediante el uso de la puerta de enlace RAS.

![Información general sobre el escenario de prueba de túnel GRE de puerta de enlace de RAS](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>Configuración del entorno de prueba

En esta sección se proporciona información sobre el entorno de prueba y la configuración de puerta de enlace RAS.

En el entorno de prueba, se implementan máquinas virtuales de puerta de enlace de RAS en Hyper\-hosts V en una conmutación por error de clúster de alta disponibilidad.

### <a name="hyper-v-host-configuration"></a>Hyper\-V configuración del Host

Dos Hyper\-hosts V están configurados para admitir el escenario de prueba de la siguiente manera. 

- Doble dos\-alojados equipos físicos están configurados con Windows Server, versión 1709
- Los dos adaptadores de red físicos en cada uno de los dos servidores están conectados a subredes diferentes: ambos representan las subredes de Intranet de una organización. Las redes y hardware auxiliar tienen una capacidad de 10 GBps.
- Hyper-Threading en los servidores físicos está deshabilitada. Esto proporciona el máximo rendimiento de las NIC físicas.
- El Hyper\-rol de servidor V se instala en ambos servidores y se configura con dos externo Hyper\-V conmutadores virtuales, uno para cada adaptador de red físico.
- Dado que ambos servidores están conectados a la misma intranet, los servidores pueden comunicarse entre sí.
- El Hyper\-hosts V se configuran en un clúster de conmutación por error a través de la red de intranet. 

>[!NOTE]
>Para obtener más información, consulte [Conmutador virtual de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch).

### <a name="vm-configuration"></a>Configuración de máquina virtual

Dos máquinas virtuales están configuradas para admitir el escenario de prueba de la siguiente manera.

- En cada servidor de que una máquina virtual está instalada que es ejecutar Windows Server, versión 1709. Cada máquina virtual se configura con 10 núcleos y 8 GB de RAM.
- Cada máquina virtual también se configura con dos adaptadores de red virtual. Un adaptador de red virtual está conectado al conmutador virtual 1 de la Intranet y el otro adaptador de red virtual está conectado al conmutador virtual 2 de la Intranet.
- Cada máquina virtual tiene la puerta de enlace de RAS instalado y configurado como un GRE\-en función de servidor VPN.
- Las máquinas virtuales de puerta de enlace se configuran en un clúster de conmutación por error. Cuando un clúster, una máquina virtual está activa y la otra máquina virtual está pasiva.

### <a name="workload-hyper-v-hosts-and-vms"></a>Carga de trabajo Hyper\-V Hosts y máquinas virtuales

Para esta prueba, la carga de trabajo de dos Hyper\-hosts V están instalados en la intranet, y cada host tiene una máquina virtual instalada. Si va a duplicar esta prueba en su propio entorno de prueba, puede instalar tantos servidores de carga de trabajo y máquinas virtuales según resulte adecuado para sus fines.

- Carga de trabajo Hyper\-hosts V tienen uno instalado que es el adaptador de red físico conectado a la Intranet de la organización.
- En Hyper\-V Virtual Switch, se crea un conmutador virtual en cada host. El conmutador es externo y está enlazado al adaptador de uno red conectado a la intranet.
- Las máquinas virtuales de carga de trabajo se configuran con 2 GB de RAM y 2 núcleos.
- Las máquinas virtuales de carga de trabajo de cada uno tiene un adaptador de red virtual que está conectado al conmutador virtual de intranet.

### <a name="traffic-generator-tool"></a>Herramienta Generador de tráfico

La herramienta de generador de tráfico que se usa en esta prueba es la herramienta ctsTraffic. El repositorio de Git para esta herramienta se encuentra en [ https://github.com/Microsoft/ctsTraffic ](https://github.com/Microsoft/ctsTraffic).

## <a name="ras-gateway-performance"></a>Rendimiento de la puerta de enlace RAS

Las ilustraciones de esta sección describen el Administrador de tareas muestra de rendimiento de túnel GRE con varias conexiones TCP.

Para lograr hasta 2.0 rendimiento Gbps en múltiples\-principales de las máquinas virtuales que se configuran como puertas de enlace de RAS de GRE.

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>Rendimiento de túnel GRE con varias sesiones TCP

Con varias sesiones TCP, el uso de la CPU alcanza el 100% y el rendimiento máximo en el túnel GRE es 2.0 Gbps.

La siguiente ilustración muestra el uso de CPU en ambas de las máquinas virtuales de puerta de enlace de RAS. La máquina virtual activa, máquina virtual de puerta de enlace RAS 1 #, está a la izquierda, mientras que la máquina virtual pasiva, máquina virtual de puerta de enlace de RAS 2 #, está a la derecha.

![Uso de CPU de la máquina virtual de puerta de enlace en el Administrador de tareas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

La siguiente ilustración muestra el rendimiento de la red Ethernet en las máquinas virtuales de puerta de enlace de RAS. La máquina virtual activa, máquina virtual de puerta de enlace RAS 1 #, está a la izquierda, mientras que la máquina virtual pasiva, máquina virtual de puerta de enlace de RAS 2 #, está a la derecha.

![Rendimiento de red Ethernet de máquina virtual de puerta de enlace en el Administrador de tareas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>Rendimiento de túnel GRE con una conexión TCP

Con la configuración de prueba de varias sesiones TCP puede cambiada a una sola sesión TCP, un solo núcleo de CPU alcanza la capacidad máxima en las máquinas virtuales de puerta de enlace de RAS.

El rendimiento máximo en el túnel GRE es entre 400 y 500 Mbps.

La siguiente ilustración muestra el uso de CPU en ambas de las máquinas virtuales de puerta de enlace de RAS. La máquina virtual activa, máquina virtual de puerta de enlace RAS 1 #, está a la izquierda, mientras que la máquina virtual pasiva, máquina virtual de puerta de enlace de RAS 2 #, está a la derecha.

![Uso de CPU de la máquina virtual de puerta de enlace en el Administrador de tareas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


La siguiente ilustración muestra el rendimiento de la red Ethernet en las máquinas virtuales de puerta de enlace de RAS. La máquina virtual activa, máquina virtual de puerta de enlace RAS 1 #, está a la izquierda, mientras que la máquina virtual pasiva, máquina virtual de puerta de enlace de RAS 2 #, está a la derecha.

![Rendimiento de red Ethernet de máquina virtual de puerta de enlace en el Administrador de tareas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

Para obtener más información sobre el rendimiento de la puerta de enlace RAS, consulte [ajuste del rendimiento de puerta de enlace de HNV en redes de Software definen](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance).