---
title: Rendimiento de túnel GRE de puerta de enlace de RAS
description: En este tema, destinado a los profesionales de tecnologías de la información (TI), se proporciona información de rendimiento sobre los túneles de encapsulación de enrutamiento genérico (GRE) de puerta de enlace RAS.
manager: brianlic
ms.prod: windows-server
ms.date: ''
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9188fc73cd65290474eb9f21342b88d0fce2e15b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308571"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>Rendimiento de túnel GRE de puerta de enlace de RAS

>Se aplica a: canal semianual de Windows Server \(\)

Puede usar este tema para obtener información sobre el servidor de acceso remoto \(encapsulación de enrutamiento genérico de puerta de enlace de RAS\) \(GRE\) rendimiento de túnel en Windows Server, versión 1709, en un entorno de prueba basado en redes no definidas por software \(SDN\).

La puerta de enlace RAS es un enrutador de software y una puerta de enlace que puede usar en el modo de un solo inquilino o en el modo multiempresa. En este tema se describe el modo de un solo inquilino, la configuración de alta disponibilidad con clústeres de conmutación por error. Las estadísticas de rendimiento del túnel GRE que se presentan en este tema son válidas para la puerta de enlace de RAS en los modos de inquilino de singele y multiinquilino.

>[!NOTE]
>Los clústeres de conmutación por error son una característica de Windows Server que permite agrupar varios servidores en un clúster tolerante a errores. Para obtener más información, consulte [clústeres de conmutación por error](../../../failover-clustering/failover-clustering-overview.md)

El modo de un solo inquilino permite a las organizaciones de cualquier tamaño implementar la puerta de enlace como una red privada virtual de perímetro o de Internet\-orientada a Internet \(VPN\) servidor. En el modo de un solo inquilino, puede implementar la puerta de enlace de RAS en un servidor físico o una máquina virtual \(\)de máquinas virtuales. En este tema se describe la implementación de puerta de enlace de RAS en dos Virtual Machines \(máquinas virtuales\) que están configuradas en un clúster de conmutación por error.

>[!IMPORTANT]
>Dado que los túneles GRE proporcionan encapsulación pero no cifrado, no debe usar la puerta de enlace RAS configurada con GRE como puerta de enlace perimetral de Internet. Para obtener información sobre los mejores usos de la puerta de enlace RAS con túneles GRE, consulte [tunelización de GRE en Windows Server](gre-tunneling-windows-server.md).

GRE es un protocolo de túnel ligero que puede encapsular una amplia variedad de protocolos de capa de red dentro de\-de punto virtual para\-vínculos de punto a través de una red de protocolo de Internet. La implementación de Microsoft GRE encapsula IPv4 e IPv6.

Para obtener más información, consulte la sección **escenarios de implementación de puerta de enlace de Ras** en el tema puerta de enlace de [ras](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy). 

En este escenario de prueba, que se representa en la siguiente ilustración, el flujo de tráfico que se mide pasa de la intranet de la organización 2 a la intranet 1 de la organización. Las máquinas virtuales de carga de trabajo de inquilino envían tráfico de red de la intranet 2 a la intranet 1 mediante la puerta de enlace RAS.

![Información general del escenario de prueba de túnel GRE de puerta de enlace RAS](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>Configuración del entorno de prueba

En esta sección se proporciona información sobre el entorno de prueba y la configuración de puerta de enlace RAS.

En el entorno de prueba, las máquinas virtuales de puerta de enlace RAS se implementan en hosts de Hyper\-V en un clúster de conmutación por error para alta disponibilidad.

### <a name="hyper-v-host-configuration"></a>Configuración del host de Hyper\-V

Dos hosts de Hyper\-V están configurados para admitir el escenario de prueba de la siguiente manera. 

- Dos equipos físicos de doble\-se configuran con Windows Server, versión 1709
- Los dos adaptadores de red físicos de cada uno de los dos servidores están conectados a subredes diferentes, y ambos representan subredes de una intranet de la organización. Ambas redes y el hardware compatible tienen una capacidad de 10 GBps.
- El hyperthreading en los servidores físicos está deshabilitado. Esto proporciona el máximo rendimiento de las NIC físicas.
- El rol de servidor de Hyper\-V se instala en ambos servidores y se configura con dos conmutadores virtuales externos de Hyper\-V, uno para cada adaptador de red físico.
- Dado que ambos servidores están conectados a la misma intranet, los servidores pueden comunicarse entre sí.
- Los hosts de Hyper\-V se configuran en un clúster de conmutación por error a través de la red de intranet. 

>[!NOTE]
>Para obtener más información, consulte [Conmutador virtual de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch).

### <a name="vm-configuration"></a>Configuración de VM

Dos máquinas virtuales están configuradas para admitir el escenario de prueba de la siguiente manera.

- En cada servidor se instala una máquina virtual que ejecuta Windows Server, versión 1709. Cada máquina virtual se configura con 10 núcleos y 8 GB de RAM.
- Cada máquina virtual también se configura con dos adaptadores de red virtuales. Un adaptador de red virtual está conectado al conmutador virtual de la intranet 1 y el otro adaptador de red virtual está conectado al conmutador virtual de la intranet 2.
- Cada máquina virtual tiene una puerta de enlace RAS instalada y configurada como un servidor VPN basado en\-GRE.
- Las máquinas virtuales de puerta de enlace se configuran en un clúster de conmutación por error. Cuando está en clúster, una máquina virtual está activa y la otra máquina virtual es pasiva.

### <a name="workload-hyper-v-hosts-and-vms"></a>Hosts y máquinas virtuales de Hyper\-V de carga de trabajo

Para esta prueba, se instalan dos hosts de Hyper\-V de carga de trabajo en la intranet y cada host tiene una máquina virtual instalada. Si va a duplicar esta prueba en su propio entorno de prueba, puede instalar tantos servidores de carga de trabajo como máquinas virtuales como sea adecuado para sus fines.

- Los hosts de Hyper\-V de carga de trabajo tienen instalado un adaptador de red físico que está conectado a la intranet de la organización.
- En el conmutador virtual de Hyper\-V, se crea un conmutador virtual en cada host. El modificador es externo y está enlazado a un adaptador de red conectado a la intranet.
- Las máquinas virtuales de carga de trabajo se configuran con 2 GB de RAM y 2 núcleos.
- Cada una de las máquinas virtuales de carga de trabajo tiene un adaptador de red virtual que está conectado al conmutador virtual de la intranet.

### <a name="traffic-generator-tool"></a>Herramienta del generador de tráfico

La herramienta de generador de tráfico que se usa en esta prueba es la herramienta ctsTraffic. El repositorio de Git de esta herramienta se encuentra en [https://github.com/Microsoft/ctsTraffic](https://github.com/Microsoft/ctsTraffic).

## <a name="ras-gateway-performance"></a>Rendimiento de puerta de enlace RAS

En las ilustraciones de esta sección se describen las pantallas del administrador de tareas de rendimiento del túnel GRE con varias conexiones TCP.

Puede conseguir un rendimiento de hasta 2,0 Gbps en máquinas virtuales de varios\-Core que están configuradas como puertas de enlace de RAS GRE.

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>Rendimiento del túnel GRE con varias sesiones TCP

Con varias sesiones TCP, el uso de CPU alcanza el 100% y el rendimiento máximo en el túnel GRE es de 2,0 Gbps.

En la ilustración siguiente se muestra el uso de CPU en las dos máquinas virtuales de puerta de enlace de RAS. La máquina virtual activa, la máquina virtual de puerta de enlace RAS #1, está a la izquierda, mientras que la máquina virtual pasiva, la máquina virtual de puerta de enlace RAS #2, está a la derecha.

![Uso de CPU de VM de puerta de enlace en el administrador de tareas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

En la ilustración siguiente se muestra el rendimiento de la red Ethernet en las máquinas virtuales de puerta de enlace RAS. La máquina virtual activa, la máquina virtual de puerta de enlace RAS #1, está a la izquierda, mientras que la máquina virtual pasiva, la máquina virtual de puerta de enlace RAS #2, está a la derecha.

![Rendimiento de red Ethernet de VM de puerta de enlace en el administrador de tareas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>Rendimiento del túnel GRE con una conexión TCP

Con la configuración de prueba modificada de varias sesiones TCP a una única sesión TCP, solo un núcleo de CPU alcanza la capacidad máxima en las máquinas virtuales de puerta de enlace RAS.

El rendimiento máximo del túnel GRE es de entre 400-500 Mbps.

En la ilustración siguiente se muestra el uso de CPU en las dos máquinas virtuales de puerta de enlace de RAS. La máquina virtual activa, la máquina virtual de puerta de enlace RAS #1, está a la izquierda, mientras que la máquina virtual pasiva, la máquina virtual de puerta de enlace RAS #2, está a la derecha.

![Uso de CPU de VM de puerta de enlace en el administrador de tareas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


En la ilustración siguiente se muestra el rendimiento de la red Ethernet en las máquinas virtuales de puerta de enlace RAS. La máquina virtual activa, la máquina virtual de puerta de enlace RAS #1, está a la izquierda, mientras que la máquina virtual pasiva, la máquina virtual de puerta de enlace RAS #2, está a la derecha.

![Rendimiento de red Ethernet de VM de puerta de enlace en el administrador de tareas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

Para obtener más información sobre el rendimiento de la puerta de enlace RAS, consulte [ajuste del rendimiento de puerta de enlace HNV en redes definidas por software](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance).