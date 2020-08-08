---
title: Funciones y tecnologías integradas de software y hardware (SH)
description: Estas características tienen componentes de software y hardware. El software está estrechamente ligado a las capacidades de hardware necesarias para que funcione la característica. Entre los ejemplos se incluyen VMMQ, VMQ, descarga de suma de comprobación IPv4 del lado de envío y RSS.
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/12/2018
ms.openlocfilehash: 3c1b414acaf7487b0a435cfea2891903646c869f
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995265"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>Funciones y tecnologías integradas de software y hardware (SH)

Estas características tienen componentes de software y hardware. El software está estrechamente ligado a las capacidades de hardware necesarias para que funcione la característica. Entre los ejemplos se incluyen VMMQ, VMQ, descarga de suma de comprobación IPv4 del lado de envío y RSS.

>[!TIP]
>Las características SH y HO están disponibles si la NIC instalada lo admite. En las descripciones de las características siguientes se explicará cómo saber si la NIC es compatible con la característica.

## <a name="converged-nic"></a>NIC convergente

La NIC convergente es una tecnología que permite a las NIC virtuales del host de Hyper-V exponer los servicios RDMA para hospedar los procesos. Windows Server 2016 ya no requiere NIC independientes para RDMA. La característica NIC convergente permite que las NIC virtuales de la partición del host (VNIC) expongan RDMA a la partición del host y compartan el ancho de banda de las NIC entre el tráfico RDMA y la máquina virtual y otro tráfico TCP/UDP de manera equitativa y administrable.

![NIC convergente con SDN](../../media/Converged-NIC/conv-nic-sdn.png)

Puede administrar la operación de NIC convergente a través de VMM o Windows PowerShell. Los cmdlets de PowerShell son los mismos cmdlets que se usan para RDMA (consulte a continuación).

Para usar la funcionalidad de NIC convergente:

1.  Asegúrese de configurar el host para DCB.

2.  Asegúrese de habilitar RDMA en la NIC o, en el caso de un equipo establecido, las NIC se enlazan al conmutador de Hyper-V.

3.  Asegúrese de habilitar RDMA en el VNIC designado para RDMA en el host.

Para obtener más detalles sobre RDMA y SET, vea [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="data-center-bridging-dcb"></a>Data Center Bridging (DCB)

DCB es un conjunto de estándares del Instituto de ingenieros de electricidad y electrónica (IEEE) que permiten tejidos convergentes en centros de datos. DCB proporciona administración de ancho de banda basada en la cola de hardware en un host con cooperación desde el conmutador adyacente. Todo el tráfico de almacenamiento, redes de datos, comunicación entre procesos (IPC) de clúster y administración comparten la misma infraestructura de red Ethernet. En Windows Server 2016, DCB puede aplicarse a cualquier NIC individualmente y a las NIC enlazadas al conmutador de Hyper-V.

En DCB, Windows Server usa el control de flujo basado en prioridades (PFC), normalizado en IEEE 802.1 QBB. PFC crea un tejido de red (casi) sin pérdida evitando el desbordamiento dentro de las clases de tráfico. Windows Server también usa la selección de transmisión mejorada (ETS), normalizada en IEEE 802.1 QAZ. ETS permite dividir el ancho de banda en partes reservadas para hasta ocho clases de tráfico. Cada clase de tráfico tiene su propia cola de transmisión y, mediante el uso de PFC, puede iniciar y detener la transmisión dentro de una clase.

Para obtener más información, vea protocolo de [puente del centro de datos (DCB)](../dcb/dcb-top.md).

## <a name="hyper-v-network-virtualization"></a>Virtualización de red de Hyper-V

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **V1 (HNVv1)**       |                     La virtualización de red de Hyper-V (HNV), que se presentó en Windows Server 2012, permite la virtualización de redes de clientes sobre una infraestructura de red física compartida. Con los cambios mínimos necesarios en el tejido de red físico, HNV ofrece a los proveedores de servicios la agilidad para implementar y migrar cargas de trabajo de inquilino en cualquier parte de las tres nubes: la nube del proveedor de servicios, la nube privada o la Microsoft Azure nube pública.                     |
| **V2 NVGRE (HNVv2 NVGRE)** | En Windows Server 2016 y System Center Virtual Machine Manager, Microsoft proporciona una solución de virtualización de red de un extremo a otro que incluye la puerta de enlace RAS, el equilibrio de carga de software, la controladora de red y mucho más. Para obtener más información, consulte [Introducción a virtualización de red de Hyper-V en Windows Server 2016](../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server.md). |
| **V2 VxLAN (HNVv2 VxLAN)** |                                                                                                                                                                                        En Windows Server 2016, forma parte de la extensión SDN, que se administra a través de la controladora de red.                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>Descarga de tareas de IPsec (IPsecto)

La descarga de tareas de IPsec es una característica de NIC que permite que el sistema operativo use el procesador en la NIC para el trabajo de cifrado de IPsec.

>[!IMPORTANT]
>La descarga de tareas de IPsec es una tecnología heredada que no es compatible con la mayoría de los adaptadores de red y donde existe, está deshabilitada de forma predeterminada.

## <a name="private-virtual-local-area-network-pvlan"></a>Red de área local virtual privada (PVLAN).

PVLAN permite la comunicación solo entre máquinas virtuales en el mismo servidor de virtualización. Una red virtual privada no está enlazada a un adaptador de red físico. Una red virtual privada se aísla de todo el tráfico de red externo en el servidor de virtualización, así como de cualquier tráfico de red entre el sistema operativo de administración y la red externa. Este tipo de red resulta útil cuando necesita crear un entorno de red aislado como, por ejemplo, un dominio de prueba aislado. Las pilas de Hyper-V y SDN solo admiten el modo de Puerto aislado PVLAN.

Para obtener más información acerca del aislamiento de PVLAN, consulte [System Center: blog de ingeniería de Virtual Machine Manager](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/).

## <a name="remote-direct-memory-access-rdma"></a>Acceso directo a memoria remota (RDMA)

RDMA es una tecnología de red que proporciona comunicación de alto rendimiento y baja latencia que minimiza el uso de la CPU. RDMA admite redes de cero copias al permitir que el adaptador de red transfiera datos directamente a o desde la memoria de la aplicación. Compatible con RDMA significa que la NIC (física o virtual) es capaz de exponer RDMA a un cliente RDMA. Por otro lado, habilitado para RDMA significa que una NIC compatible con RDMA expone la interfaz RDMA hacia arriba en la pila.

Para obtener más detalles sobre RDMA, consulte [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS)

RSS es una característica de NIC que separa diferentes conjuntos de secuencias y los entrega a diferentes procesadores para su procesamiento. RSS paralelizan el procesamiento de redes, lo que permite que un host escale a tarifas de datos muy altas.

Para obtener más información, vea [ajuste de escala en lado de recepción (RSS)](/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

## <a name="single-root-input-output-virtualization-sr-iov"></a>Virtualización de entrada y salida de raíz única (SR-IOV)

SR-IOV permite que el tráfico de la máquina virtual se mueva directamente de la NIC a la máquina virtual sin pasar por el host de Hyper-V. SR-IOV es una mejora increíble en el rendimiento de una máquina virtual, pero carece de la capacidad para que el host administre esa canalización. Use SR-IOV solo cuando la carga de trabajo esté bien comportada, sea de confianza y, por lo general, la única máquina virtual del host.

El tráfico que usa SR-IOV omite el conmutador de Hyper-V, lo que significa que no se aplicarán las directivas, por ejemplo, las ACL o la administración del ancho de banda. El tráfico de SR-IOV tampoco se puede pasar a través de ninguna funcionalidad de virtualización de red, por lo que no se puede aplicar la encapsulación NV-GRE o VxLAN. Use SR-IOV solo para cargas de trabajo de confianza en situaciones específicas. Además, no puede usar las directivas de host, la administración de ancho de banda y las tecnologías de virtualización.

En el futuro, dos tecnologías permitirían SR-IOV: tablas de flujo genérico (GFT) y descarga de QoS de hardware (administración de ancho de banda en la NIC): una vez que las NIC del ecosistema las admitan. La combinación de estas dos tecnologías haría que SR-IOV fuera útil para todas las máquinas virtuales, permitiría aplicar directivas, virtualización y reglas de administración de ancho de banda, y podría dar lugar a grandes saltos hacia delante en la aplicación general de SR-IOV.

Para obtener más información, vea [información general de la virtualización de e/s de raíz única (SR-IOV)](/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-).

## <a name="tcp-chimney-offload"></a>Descarga de TCP Chimney

La descarga TCP Chimney, también conocida como descarga de motor TCP (TOE), es una tecnología que permite al host descargar todo el procesamiento TCP a la NIC. Dado que la pila TCP de Windows Server es casi siempre más eficaz que el motor TOE, no se recomienda el uso de la descarga TCP Chimney.

>[!IMPORTANT]
>La descarga TCP Chimney es una tecnología desusada. Se recomienda no usar la descarga TCP Chimney, ya que es posible que Microsoft deje de admitirla en el futuro.

## <a name="virtual-local-area-network-vlan"></a>Red de área local virtual (VLAN)

VLAN es una extensión del encabezado de fotogramas Ethernet para habilitar la creación de particiones de una LAN en varias VLAN, cada una con su propio espacio de direcciones. En Windows Server 2016, las VLAN se establecen en los puertos del conmutador de Hyper-V o mediante la configuración de las interfaces de equipo en los equipos de formación de equipos NIC. Para obtener más información, consulte [formación de equipos NIC y redes de área local virtual (VLAN)](../nic-teaming/nic-teaming.md).

## <a name="virtual-machine-queue-vmq"></a>Virtual Machine Queue (VMQ)

VMQ es una característica de NIC que asigna una cola para cada máquina virtual. Siempre que haya habilitado Hyper-V; también debe habilitar VMQ. En Windows Server 2016, VMQ usar el conmutador NIC vPorts con una sola cola asignada a vPort para proporcionar la misma funcionalidad. Para obtener más información, vea [ajuste de escala en lado de recepción virtual (vRSS)](../vrss/vrss-top.md) y formación de [equipos NIC](../nic-teaming/nic-teaming.md).

## <a name="virtual-machine-multi-queue-vmmq"></a>Varias colas de máquinas virtuales (VMMQ)

VMMQ es una característica de NIC que permite que el tráfico de una máquina virtual se reparta entre varias colas, cada una procesada por un procesador físico diferente. Después, el tráfico se pasa a varios LPs en la máquina virtual tal como lo haría en vRSS, lo que permite entregar un ancho de banda de red considerable a la máquina virtual.

---