---
title: Funciones y tecnologías integradas de software y hardware (SH)
description: Estas características tienen componentes de hardware y software. El software está estrechamente vinculado a las capacidades de hardware que son necesarias para que funcione la característica. Ejemplos de estas incluyen VMMQ, VMQ, la descarga de suma de comprobación de IPv4 de envío y RSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 98857a5a6d665728c1aab2a6a2df64997d4166b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446224"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>Funciones y tecnologías integradas de software y hardware (SH)

Estas características tienen componentes de hardware y software. El software está estrechamente vinculado a las capacidades de hardware que son necesarias para que funcione la característica. Ejemplos de estas incluyen VMMQ, VMQ, la descarga de suma de comprobación de IPv4 de envío y RSS.

>[!TIP]
>SH y HO características están disponibles si la NIC instalada lo admite. Las siguientes descripciones de característica tratará cómo saber si la NIC es compatible con la característica.

## <a name="converged-nic"></a>NIC convergente 

Convergente NIC es una tecnología que permite a las NIC virtuales en el host de Hyper-V para exponer los servicios RDMA para procesos de host. Windows Server 2016 ya no requiere NIC independientes para RDMA. La característica convergente NIC permite la NIC Virtual en la partición del Host (VNIC) para exponer RDMA a la partición del host y comparten el ancho de banda de las NIC entre el tráfico RDMA y la máquina virtual y el tráfico TCP/UDP de manera razonable y fáciles de administrar.

![NIC convergente con SDN](../../media/Converged-NIC/conv-nic-sdn.png)

Puede administrar convergente NIC de operación a través de VMM o Windows PowerShell. Los cmdlets de PowerShell son los mismos cmdlets usados para RDMA (ver abajo).

Para usar la funcionalidad NIC convergente:

1.  Asegúrese de establecer el host para DCB.

2.  Asegúrese de habilitar RDMA en la NIC o, en el caso de un equipo de conjunto, las NIC están enlazadas al conmutador de Hyper-V. 

3.  Asegúrese de habilitar RDMA en las VNIC designadas para RDMA en el host. 

Para obtener más información acerca de RDMA y SET, vea [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="data-center-bridging-dcb"></a>Protocolo de puente del centro de datos (DCB) 

DCB es un conjunto de estándares del Instituto de Electrical and Electronics Engineers (IEEE) que permiten a usar tejidos convergentes en centros de datos. DCB proporciona administración de ancho de banda basada en la cola de hardware en un host con la cooperación del conmutador adyacente. Todo el tráfico de almacenamiento, redes, datos de comunicación entre procesos (IPC) del clúster y la administración comparten la misma infraestructura de red Ethernet. En Windows Server 2016, DCB puede aplicarse a cualquier NIC individualmente y al NIC enlazadas a la función Hyper-V switch.

Para DCB, según la prioridad de flujo de Control (PFC), normalizado en IEEE 802.1Qbb usa Windows Server. PFC crea a un tejido de red (casi) sin pérdida de datos evitando desbordamiento dentro de las clases de tráfico. También usa mejorado transmisión selección (ETS), normalizado en IEEE 802.1Qaz en Windows Server. ETS permite la división del ancho de banda en partes reservadas para un máximo de ocho clases de tráfico. Cada clase de tráfico tiene su propia transmisión en cola y, a través del uso PFC, puede iniciar y detener transmisión dentro de una clase.

Para obtener más información, consulte [Data Center Bridging (DCB)](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top).

## <a name="hyper-v-network-virtualization"></a>Virtualización de red de Hyper-V

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **v1 (HNVv1)**       |                     Se introdujo en Windows Server 2012, virtualización de red de Hyper-V (HNV) permite la virtualización de redes de cliente en una infraestructura de red física compartida. Con unos cambios mínimos necesarios en el tejido de red físico, HNV ofrece proveedores de servicios de la agilidad necesaria para implementar y migrar las cargas de trabajo de inquilino en cualquier lugar entre las tres nubes: la nube del proveedor de servicio, la nube privada o la nube pública de Microsoft Azure.                     |
| **NVGRE v2 (HNVv2 NVGRE)** | En Windows Server 2016 y System Center Virtual Machine Manager, Microsoft ofrece una solución de virtualización de red to-end que incluye la puerta de enlace RAS, equilibrio de carga de Software, controlador de red y mucho más. Para obtener más información, consulte [descripción general de virtualización de red de Hyper-V en Windows Server 2016](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server). |
| **v2 VxLAN (HNVv2 VxLAN)** |                                                                                                                                                                                        En Windows Server 2016, forma parte de la extensión de SDN, que se administra a través de la controladora de red.                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>Descarga de tareas de IPsec (IPsecTO) 

Descarga de tareas de IPsec es una característica NIC que permite al sistema operativo usar el procesador en la NIC para el trabajo de cifrado de IPsec.

>[!IMPORTANT] 
>Descarga de tareas de IPsec es una tecnología heredada que no es compatible con la mayoría de los adaptadores de red, y donde existe, está deshabilitado de forma predeterminada.

## <a name="private-virtual-local-area-network-pvlan"></a>Privado virtual red de área Local (PVLAN). 

Pvlan permite la comunicación únicamente entre las máquinas virtuales en el mismo servidor de virtualización. Una red virtual privada no está enlazada a un adaptador de red físico. Una red virtual privada se aísla de todo el tráfico de red externo en el servidor de virtualización, así como cualquier tráfico de red entre el sistema operativo de administración y la red externa. Este tipo de red resulta útil cuando necesita crear un entorno de red aislado como, por ejemplo, un dominio de prueba aislado. El Hyper-V y las pilas de SDN admiten solo en modo puerto aislado de PVLAN.

Para obtener más información sobre el aislamiento de PVLAN, consulte [System Center: Blog de ingeniería de Virtual Machine Manager](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/).

## <a name="remote-direct-memory-access-rdma"></a>Acceso directo a memoria remota (RDMA) 

RDMA es una tecnología de red que permite la comunicación de alto rendimiento y baja latencia que minimiza el uso de CPU. RDMA es compatible con copia de cero redes habilitando el adaptador de red transferir datos directamente a o desde la memoria de la aplicación. Compatibles con RDMA significa que la NIC (física o virtual) es capaz de exponer RDMA a un cliente RDMA. Habilitados para RDMA, por otro lado, significa que una NIC compatible con RDMA expone la interfaz RDMA arriba en la pila.

Para obtener más información acerca de RDMA, consulte [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

RSS es una característica NIC que aísla los diferentes conjuntos de secuencias y los entrega a procesadores diferentes para su procesamiento. RSS paraleliza el procesamiento, lo que permite un host escalar a muy alta velocidad de datos de la red. 

Para obtener más información, consulte [escalado de lado de recepción (RSS)](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

## <a name="single-root-input-output-virtualization-sr-iov"></a>Virtualización de entrada y salida de raíz única (SR-IOV) 

SR-IOV permite el tráfico de máquina virtual mover directamente de la NIC a la máquina virtual sin pasar por el host de Hyper-V. SR-IOV es una mejora del rendimiento de una máquina virtual increíble, pero no tiene la posibilidad de que el host para administrar esa canalización. Solo use SR-IOV cuando la carga de trabajo es buen, de confianza y generalmente en la máquina virtual sola en el host.

El tráfico que utiliza SR-IOV omite el conmutador Hyper-V, lo que las directivas, por ejemplo, las ACL, o no se aplicará la administración de ancho de banda. Tráfico de SR-IOV también no se puede pasar a través de cualquier funcionalidad de virtualización de red, por lo que no se puede aplicar la encapsulación de NV GRE o VxLAN. Solo use SR-IOV para cargas de trabajo y de confianza en situaciones específicas. Además, no se puede usar las directivas de host, administración de ancho de banda y las tecnologías de virtualización.

En el futuro, las dos tecnologías permitiría SR-IOV: Genéricas de flujo de las tablas (GFT) y descarga de QoS de Hardware (administración de ancho de banda en la NIC): una vez que las NIC en nuestro ecosistema admiten. La combinación de estas dos tecnologías haría que permitiría que SR-IOV útil para todas las máquinas virtuales, directivas, virtualización y las reglas de administración de ancho de banda que se aplicará y podría resultado en grandes avances reenviar en general de la aplicación de SR-IOV.

Para obtener más información, consulte [información general de raíz de virtualización de E/S única (SR-IOV)](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-).

## <a name="tcp-chimney-offload"></a>Descarga TCP Chimney

La descarga TCP Chimney, también conocido como TCP motor de descarga (TOE), es una tecnología que permite al host la descarga TCP todo procesamiento a la NIC. Dado que la pila de TCP de Windows Server casi siempre es más eficaz que el motor TOE, la descarga TCP Chimney no se recomienda usar.

>[!IMPORTANT]
>La descarga Chimenea TCP es una tecnología en desuso. Se recomienda que no utilizar la descarga TCP Chimney como Microsoft podría dejar de admitir en el futuro.

## <a name="virtual-local-area-network-vlan"></a>Red de área Local virtual (VLAN) 

VLAN es una extensión para el encabezado de marco de Ethernet para habilitar la creación de particiones de una LAN en varias VLAN, cada uno con su propio espacio de direcciones. En Windows Server 2016, las VLAN se establecen en los puertos del conmutador de Hyper-V o mediante el establecimiento de las interfaces del equipo en los equipos de formación de equipos NIC. Para obtener más información, consulte [formación de equipos NIC y redes de área Local Virtual (VLAN)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans).

## <a name="virtual-machine-queue-vmq"></a>Virtual Machine Queue (VMQ) 

Vmq es una característica NIC que se asigna una cola para cada máquina virtual. Siempre que tenga habilitado; de Hyper-V También debe habilitar VMQ. En Windows Server 2016, Vmq usa puertos de conmutador de NIC virtuales con una sola cola asignada a la vPort para proporcionar la misma funcionalidad. Para obtener más información, consulte [Virtual escala de recepción (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top) y [formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="virtual-machine-multi-queue-vmmq"></a>Máquina virtual Multicola (VMMQ) 

VMMQ es una característica NIC que permita el tráfico de una máquina virtual distribuir en varias colas, cada una procesa un procesador físico diferente. El tráfico, a continuación, se pasa a LPs varias en la máquina virtual como lo estaría en vRSS, lo que permite entregar el ancho de banda de red considerable a la máquina virtual.

---