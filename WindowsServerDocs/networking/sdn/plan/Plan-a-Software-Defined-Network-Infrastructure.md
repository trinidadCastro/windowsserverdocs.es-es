---
title: Planeación de una infraestructura de red definida por software
description: En este tema se proporciona información sobre cómo planear la implementación de la infraestructura de red definida por Software (SDN).
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: 16511628e979a95433360b0a3e900a89e9ba56eb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446276"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planeación de una infraestructura de red definida por software

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Obtenga información sobre los planes de implementación de una infraestructura de red definida por Software, incluidos los requisitos previos de hardware y software. 


## <a name="prerequisites"></a>Requisitos previos
En este tema se describe una serie de requisitos previos de hardware y software, incluidas:

-   **Configurar grupos de seguridad, las ubicaciones de archivo de registro y el registro DNS dinámico** debe preparar su centro de datos para la implementación de controladora de red, lo que requiere uno o más equipos o máquinas virtuales y un equipo o máquina virtual. Antes de poder implementar la controladora de red, debe configurar los grupos de seguridad, las ubicaciones de archivo de registro (si es necesario) y el registro DNS dinámico.  Si no ha preparado su centro de datos para la implementación de controladora de red, consulte [instalación y requisitos de preparación para la implementación de controladora de red](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) para obtener más información.

-   **Red física** necesita acceder a los dispositivos de red físicos para configurar las VLAN, enrutamiento, BGP, Data Center de puente (ETS) si usa una tecnología RDMA, y Data Center puente (PFC) si usa un RoCE en función de la tecnología RDMA. Este tema muestra la configuración del conmutador manual así como el emparejamiento BGP en conmutadores de nivel 3 / enrutadores o una máquina virtual de enrutamiento y el servidor de acceso remoto (RRAS).   

-   **Hosts de proceso físicos** estos hosts ejecutan Hyper-V y son necesarios a las máquinas de virtuales de inquilinos e infraestructura SDN host.  Se requiere hardware de red específicas en estos hosts para mejorar el rendimiento, como se describe más adelante en el **hardware de red** sección.  


## <a name="physical-network-and-compute-host-configuration"></a>Configuración de host de proceso y red física

Cada host de proceso físicos requiere conectividad de red a través de uno o más adaptadores de red conectados a un puerto o puertos de conmutador físico.  Nivel 2 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN) es compatible con las redes que se divide en varios segmentos de red lógica. 

>[!TIP]
>Usar VLAN 0 para redes lógicas en modo de acceso o no etiquetado. 

>[!IMPORTANT]
>Windows Server 2016 redes definidas por Software es compatible con direccionamiento IPv4 para el subposición y la superposición. No se admite IPv6.

### <a name="logical-networks"></a>Redes lógicas

#### <a name="management-and-hnv-provider"></a>Administración y el proveedor de HNV 

Todos los hosts de proceso físico deben tener acceso a la red lógica de administración y la red lógica del proveedor de HNV.  Dirección de IP con fines de planificación, cada host de proceso físico debe tener al menos una dirección IP asignada desde la red lógica de administración. La controladora de red requiere una dirección IP reservada para que actúe como la dirección IP de REST. 

Un servidor DHCP puede asignar automáticamente direcciones IP para la red de administración, o puede asignar manualmente direcciones IP estáticas. La pila de SDN asigna automáticamente direcciones IP para el proveedor de HNV red lógica de los hosts de Hyper-V individuales desde un grupo de IP especificados a través y administrados por la controladora de red. 

>[!NOTE]
>La controladora de red asigna una dirección IP de proveedor de HNV a un host de proceso físicos solo después de que el agente de Host del controlador de red recibe la directiva de red para una máquina virtual de inquilino específico. 


|                                                               Si...                                                               |                                                                                                                                                                          En ese caso...                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                  Usan VLAN, en las redes lógicas                                                  |                                                                 el host de proceso físicos debe conectarse a un puerto de conmutador troncal que tiene acceso a estas redes VLAN. Es importante tener en cuenta que los adaptadores de red físico en el host de equipo no deben tener ningún filtrado de VLAN activado.                                                                 |
|                Usa Switched-Embedded Teaming (SET) y tiene varios miembros del equipo NIC, como los adaptadores de red,                |                                                                                                                        debe conectar todos los miembros del equipo NIC para ese host específico para el mismo dominio de difusión de nivel 2.                                                                                                                         |
| El host de proceso físicos se está ejecutando las máquinas virtuales de infraestructura adicional, como puerta de enlace, SLB/MUX o controladora de red | ese host debe tener una dirección IP adicional asignada desde la red lógica de administración para cada una de las máquinas virtuales hospedadas.<p>Además, cada máquina virtual de SLB/MUX infraestructura debe tener una dirección IP reservada para la red lógica del proveedor de HNV. Error al tener una dirección IP reservada puede dar lugar a direcciones IP duplicadas en la red. |

---

Para obtener información acerca de la virtualización de red de Hyper-V (HNV), que puede usar para la virtualización de redes en una implementación de SDN de Microsoft, consulte [virtualización de red de Hyper-V](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  



#### <a name="gateways-and-the-software-load-balancer"></a>Las puertas de enlace y el equilibrador de carga de Software

Redes lógicas adicionales necesitan se creará y aprovisionará para puerta de enlace y el uso SLB. Asegúrese de obtener los prefijos IP correctos, Id. de VLAN y las direcciones IP de puerta de enlace para estas redes.


|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **Red lógica de tránsito**   | La puerta de enlace RAS y SLB/MUX utilizan la red lógica de tránsito para intercambiar información de emparejamiento BGP y tráfico de inquilinos (external interno) de norte/sur. El tamaño de esta subred normalmente será menor que los demás. Solo los hosts de proceso físicos que ejecuten puerta de enlace RAS o máquinas virtuales SLB/MUX deben tener conectividad a esta subred con estas redes VLAN troncal y es accesible en los puertos de conmutador al que están conectados los adaptadores de red de los host de proceso. Cada SLB/MUX o máquina virtual de puerta de enlace RAS estáticamente se asigna una dirección IP desde la red lógica de tránsito. |
| **Red lógica de VIP pública**  |                                                                                                                             La red lógica de VIP pública debe tener los prefijos de subred IP sean enrutables fuera del entorno de nube (normalmente Internet enrutable).  Estos serán las direcciones IP front-end utilizadas por los clientes externos para tener acceso a recursos en las redes virtuales, incluido el front-end de dirección VIP para la puerta de enlace de sitio a sitio.                                                                                                                             |
| **Red lógica de VIP privada** |                                                                                                                                                                                       La red lógica de VIP privada no es necesaria para poder enrutarse fuera de la nube, ya que se utiliza para las VIP que sólo se tiene acceso desde clientes internos en la nube, como el Administrador de SLB o servicios privada.                                                                                                                                                                                       |
|   **Red lógica VIP GRE**   |                                                                                                                                           La red VIP GRE es una subred que existe únicamente para definir las VIP que se asignan a las máquinas virtuales de puerta de enlace que se ejecutan en el tejido SDN para un tipo de conexión GRE S2S. Esta red no deben configurarse previamente en los conmutadores físicos o el enrutador y no deben tener una VLAN asignada.                                                                                                                                            |

---


#### <a name="sample-network-topology"></a>Topología de red de ejemplo
Cambiar los prefijos de subred IP de ejemplo y los identificadores de VLAN para su entorno. 


| **Nombre de red** |  **Subnet**  | **Máscara** | **Id. de VLAN en un camión** | **Gateway**  |                                                           **Reservas de direcciones (ejemplos)**                                                           |
|------------------|--------------|----------|----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|    Management    | 10.184.108.0 |    24    |          7           | 10.184.108.1 | 10.184.108.1 – proceso de red Controller10.184.108.10 - proceso host 110.184.108.11 - Router10.184.108.4 - hospedar 210.184.108.X - host de proceso X |
|   Proveedor de HNV   |  10.10.56.0  |    23    |          11          |  10.10.56.1  |                                                    10.10.56.1 – Router10.10.56.2 - SLB/MUX1                                                     |
|     Tránsito      |  10.10.10.0  |    24    |          10          |  10.10.10.1  |                                                               10.10.10.1 – enrutador                                                               |
|    VIP pública    |  41.40.40.0  |    27    |          N/A          |  41.40.40.1  |                                    41.40.40.1 – Router41.40.40.2 SLB/MUX VIP41.40.40.3 - IPSec S2S VPN VIP                                    |
|   VIP privada    |  20.20.20.0  |    27    |          N/A          |  20.20.20.1  |                                                        20.20.20.1 - puerta de enlace predeterminada (enrutador)                                                         |
|     VIP GRE      |  31.30.30.0  |    24    |          N/A          |  31.30.30.1  |                                                             31.30.30.1 - puerta de enlace predeterminada                                                             |

---

### <a name="logical-networks-required-for-rdma-based-storage"></a>Redes lógicas necesarias para almacenamiento basado en RDMA  

Si usa almacenamiento basado en RDMA, defina una VLAN y subred para cada adaptador físico (dos adaptadores por nodo) en los hosts de proceso y almacenamiento.  

>[!IMPORTANT]
>Calidad de servicio (QoS) para que se hayan aplicado, conmutadores físicos requieren una etiquetado VLAN para el tráfico RDMA.

| **Nombre de red** |  **Subnet**  | **Máscara** | **Id. de VLAN en un camión** | **Gateway**  |                                                           **Reservas de direcciones (ejemplos)**                                                            |
|------------------|--------------|----------|----------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
|     Storage1     |  10.60.36.0  |    25    |          8           |  10.60.36.1  |  10.60.36.1 – enrutador<p>10.60.36.X - host de proceso X<p>10.60.36.Y - host Y de proceso<p>10.60.36.V - clúster de proceso<p>10.60.36.W - clúster de almacenamiento  |
|     Storage2     | 10.60.36.128 |    25    |          9           | 10.60.36.129 | 10.60.36.129 – enrutador<p>10.60.36.X - host de proceso X<p>10.60.36.Y - host Y de proceso<p>10.60.36.V - clúster de proceso<p>10.60.36.W - clúster de almacenamiento |

---


## <a name="routing-infrastructure"></a>Infraestructura de enrutamiento  

Si va a implementar la infraestructura de SDN mediante scripts, la administración, el proveedor de HNV, el tránsito, y las subredes de VIP deben ser enrutables entre sí en la red física.     

Información de enrutamiento \(, por ejemplo, próximo salto\) para la dirección VIP subredes se anuncia el SLB/MUX y puertas de enlace de RAS en la red física mediante el emparejamiento de BGP interno. Las redes lógicas de VIP no tienen una VLAN asignada y no está configuradas previamente en el conmutador de capa 2 (por ejemplo, conmutador Top of Rack).  

Deberá crear a un emparejamiento BGP en el enrutador que es utilizado por la infraestructura de SDN para recibir las rutas para las redes lógicas de VIP anunciadas por el SLB/MUX y puertas de enlace de RAS. Emparejamiento de BGP solo necesita que se produzca una forma (de SLB/MUX o puerta de enlace RAS externo BGP del mismo nivel).  Por encima de la primera capa de enrutamiento se puede utilizar rutas estáticas u otro protocolo de enrutamiento dinámico como OSPF, sin embargo, como se mencionó anteriormente, el prefijo de subred IP para las redes lógicas de VIP tienen que enrutarse desde la red física para el par BGP externo.   

Normalmente, se configura el emparejamiento de BGP en un conmutador o enrutador gestionado como parte de la infraestructura de red. El par BGP también puede configurarse en un servidor de Windows con el rol de servidor de acceso remoto (RAS) instalado en un modo de enrutamiento de solo. Este enrutador de BGP en la infraestructura de red debe configurarse para tener su propio valor ASN y permitir emparejamiento de un ASN que se asigna a los componentes SDN \(SLB/MUX y puertas de enlace de RAS\). Desde el enrutador físico, o desde el Administrador de red en el control de dicho enrutador se debe obtener la siguiente información:

- ASN de enrutador  
- Dirección IP del enrutador  
- ASN para su uso por los componentes de SDN (puede ser cualquier número de AS desde el intervalo ASN privado)

>[!NOTE]
>ASN de cuatro bytes no se admite el SLB/MUX. Debe asignar ASN de dos bytes para el SLB/MUX y el enrutador wo que se conecta. Puede usar ASN de 4 bytes en otro lugar en su entorno.  

Usted o su administrador de red debe configurar el enrutador de BGP para aceptar conexiones desde el ASN y la dirección IP o la dirección de subred de la red lógica de tránsito que usen la puerta de enlace RAS y SLB/MUX.

Para obtener más información, consulte [Border Gateway Protocol (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

## <a name="default-gateways"></a>Puertas de enlace predeterminadas
Las máquinas que están configuradas para conectarse a varias redes, como los hosts físicos y máquinas virtuales de puerta de enlace solo deben tener configurada una puerta de enlace predeterminada. Configurar la puerta de enlace predeterminada en el adaptador usa para conectarse a Internet.

Las máquinas virtuales, siga estas reglas para decidir qué red que se usará como la puerta de enlace predeterminada:

1. Si una máquina virtual se conecta a la red de tránsito, o si es múltiple a la red de tránsito o cualquier otra red, use la red lógica de tránsito como puerta de enlace predeterminada.
2. Si una máquina virtual solo se conecta a la red de administración, use la red de administración como la puerta de enlace predeterminada. 
3. Use la red del proveedor de HNV de SLB/MUX y puertas de enlace de RAS. No use la red del proveedor de HNV como una puerta de enlace predeterminada. 
4. No se conectan las máquinas virtuales directamente a las redes Storage1, Storage2, VIP pública o privada.

Para hosts de Hyper-V y los nodos de almacenamiento, use la red de administración como la puerta de enlace predeterminada.  Las redes de almacenamiento nunca deben tener asignada una puerta de enlace predeterminada.


## <a name="network-hardware"></a>Hardware de red

Puede usar las siguientes secciones para planear la implementación de hardware de red.

### <a name="network-interface-cards-nics"></a>Tarjetas de interfaz de red (NIC)

Las tarjetas de interfaz de red (NIC) utilizadas en los hosts de Hyper-V y hosts de almacenamiento requieren capacidades específicas para lograr el mejor rendimiento. 

Acceso de memoria directa remota (RDMA) es un núcleo de omitir la técnica que permite transferir grandes cantidades de datos sin utilizar el host de la CPU, lo que libera la CPU para llevar a cabo otro trabajo. 

Switch Embedded Teaming (SET) es una alternativa soluciones de formación de equipos NIC que puede usar en entornos que incluyen Hyper-V y la pila de redes definidas por Software (SDN) en Windows Server 2016. CONJUNTO integra alguna funcionalidad de formación de equipos NIC en el conmutador Virtual de Hyper-V. 

Para obtener más información, consulte [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).   

Para tener en cuenta la sobrecarga al tráfico de red virtual de inquilino causado por VXLAN o NVGRE encabezados de encapsulación, la MTU de la red del tejido de nivel 2 (conmutadores y hosts) debe establecerse como mayor o igual que los Bytes 1674 \(incluidos Ethernet de capa 2 encabezados\). 

Las NIC compatibles con el nuevo *EncapOverhead* palabra clave avanzadas del adaptador establece la MTU automáticamente a través de la controladora de red del agente de Host. Las NIC que no admiten el nuevo *EncapOverhead* palabra clave es necesario establecer el tamaño de MTU manualmente en cada host físico con el *JumboPacket* \(o equivalente\) palabra clave. 


### <a name="switches"></a>Modificadores

Al seleccionar un conmutador físico y el enrutador para su entorno, asegúrese de que es compatible con el siguiente conjunto de funcionalidades:  

- Configuración de Switchport MTU \(necesarios\)  
- MTU establecido en > = Bytes 1674 \(incluir el encabezado L2 Ethernet\)  
- Protocolos de L3 \(necesarios\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-basado ECMP

Las implementaciones deben admitir las instrucciones deben en los siguientes estándares IETF.

- RFC 2545: "Extensions multiprotocolo BGP-4 para el enrutamiento de IPv6 entre dominios"  
- RFC 4760: "Multiprotocolo Extensions para BGP-4"  
- RFC 4893: "Compatibilidad con BGP AS cuatro octetos numerar el espacio"  
- RFC 4456: "Ruta BGP reflexión: Una alternativa a la malla completa, BGP interno (IBGP)"  
- RFC 4724: "Mecanismo de reinicio normal para BGP"  

Los siguientes protocolos de etiquetado son necesarios.

- VLAN - aislamiento de distintos tipos de tráfico
- 802.1Q tronco

Los siguientes elementos proporcionan control de vínculo.

- Calidad de servicio \(PFC necesario solo si se usa RoCE\)
- Mejorado tráfico selección \(802.1Qaz\)
- Control de flujo basado en prioridades \(802.1p/Q y 802.1Qbb\)

Los siguientes elementos proporcionan redundancia y disponibilidad.

- Disponibilidad del conmutador (obligatorio)
- Se requiere un enrutador de alta disponibilidad para realizar funciones de puerta de enlace. Puede hacerlo mediante el uso de tecnologías como VRRP o un enrutador de chasis múltiples switch\.

Los siguientes elementos proporcionan capacidades de administración.

**Supervisión**

- SNMP v1 o v2 SNMP (obligatorio si usa la controladora de red para la supervisión del conmutador físico)  
- MIB SNMP \(necesarios si va a utilizar controladora de red para la supervisión del conmutador físico\)  
- MIB-II (RFC 1213), LLDP, la interfaz MIB \(RFC 2863\), IF-MIB, IP MIB, MIB de REENVÍO de IP, Q MIB de puente, MIB de puente, LLDB MIB, MIB de entidad, IEEE8023 MIB de retraso  

Los diagramas siguientes muestran una configuración de cuatro nodos de ejemplo. Por motivos de claridad, el primer diagrama muestra solo el controlador de red, el segundo muestra la controladora de red y el equilibrador de carga de software y el tercer diagrama muestra la controladora de red, equilibrador de carga de software y la puerta de enlace.  

Estos diagramas muestran no VNIC y redes de almacenamiento. Si planea usar almacenamiento basado en SMB, son necesarios.

Las máquinas virtuales de la infraestructura y el inquilino se pueden distribuir en cualquier host de proceso físicos (suponiendo que existe la conectividad de red correcta para las redes lógicas correctas).  



## <a name="switch-configuration-examples"></a>Ejemplos de configuración del conmutador  

Para ayudar a configurar el conmutador físico o un enrutador, un conjunto de archivos de configuración de ejemplo para una variedad de modelos de conmutador y los proveedores están disponibles en el [repositorio de Github de SDN de Microsoft](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Un archivo Léame detallado y comandos probado interfaz de línea de comandos (CLI) para los modificadores específicos se muestran.         


## <a name="compute"></a>Cálculo  
Todos los hosts de Hyper-V deben tener instalado Windows Server 2016, Hyper-V habilitado y un conmutador virtual externo de Hyper-V creado con al menos un adaptador físico conectado a la red lógica de administración. El host debe ser accesible a través de una dirección IP de administración asignada a la vNIC de Host de administración.  

Se puede usar cualquier tipo de almacenamiento es compatible con Hyper-V, local o compartida.   

> [!TIP]  
> Es conveniente si usa el mismo nombre para todos los conmutadores virtuales, pero no es obligatorio. Si va a implementar con secuencias de comandos, vea el comentario asociado a la `vSwitchName` variable en el archivo config.psd1.  

**Requisitos de proceso de host**  
La siguiente tabla muestra los requisitos mínimos de hardware y software para los cuatro hosts físicos que utiliza en la implementación de ejemplo.  

Host|Requisitos de hardware|Requisitos de software|  
--------|-------------------------|-------------------------  
|Host de Hyper-v físico|4 núcleos a 2,66 GHz CPU<br /><br />32 GB de RAM<br /><br />300 GB de espacio de disco<br /><br />1 Gb/s (o más rápido) adaptador de red físico|OS: Windows Server 2016<br /><br />Rol de Hyper-V instalado|  


**Requisitos del rol de máquina virtual de infraestructura SDN**  

Rol|requisitos de vCPU|Requisitos de memoria|Requisitos de disco|  
--------|---------------------|-----------------------|---------------------  
|Controladora de red (tres nodos)|4 vCPU|4 GB mínimo (se recomiendan 8 GB)|75 GB para la unidad del sistema operativo  
|SLB/MUX (tres nodos)|8 vCPU|Aunque se recomiendan 8 GB|75 GB para la unidad del sistema operativo  
|Puerta de enlace RAS<br /><br />(solo grupo de puertas de enlace de tres nodos, dos activo, uno pasivo)|8 vCPU|Aunque se recomiendan 8 GB|75 GB para la unidad del sistema operativo  
|Enrutador de BGP de puerta de enlace de RAS para el emparejamiento de SLB/MUX<br /><br />(o bien usar conmutador ToR como enrutador BGP)|2 vCPU|2 GB|75 GB para la unidad del sistema operativo|  


Si usa VMM para la implementación, recursos de máquina virtual de infraestructura adicionales son necesarios para VMM y otras infraestructuras que no son de SDN. Para obtener más información, consulte [recomendaciones de Hardware mínimo para System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  

## <a name="extending-your-infrastructure"></a>Ampliar la infraestructura  
Los requisitos de tamaño y recursos para su infraestructura dependen de las máquinas virtuales de carga de trabajo de inquilino que va a hospedar. La CPU, memoria y los requisitos de disco para las máquinas virtuales de infraestructura (por ejemplo: puerta de enlace de controlador, SLB, red, etc.) se muestran en la tabla anterior. Puede agregar varias de estas máquinas virtuales de infraestructura para escalar horizontalmente según sea necesario. Sin embargo, las máquinas virtuales de inquilinos que se ejecutan en los hosts de Hyper-V tiene su propia CPU, memoria y los requisitos de disco que debe tener en cuenta.   

Cuando las máquinas virtuales de carga de trabajo de inquilino empieza a consumir demasiados recursos en los hosts de Hyper-V físicos, puede ampliar su infraestructura mediante la adición de hosts físicos adicionales. Esto puede hacerse con Virtual Machine Manager o mediante scripts de PowerShell (dependiendo de cómo haya implementado inicialmente la infraestructura) para crear nuevos recursos de servidor a través de la controladora de red. Si necesita agregar más direcciones IP para la red del proveedor de HNV, puede crear nuevas subredes lógicas (con grupos de IP correspondiente) que pueden usar los hosts.  


## <a name="see-also"></a>Vea también  
[Instalación y los requisitos de preparación para la implementación de controladora de red](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Redes definidas por software &#40;SDN&#41;](../Software-Defined-Networking--SDN-.md)  



