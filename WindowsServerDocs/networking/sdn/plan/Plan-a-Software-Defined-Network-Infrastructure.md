---
title: Planeación de una infraestructura de red definida por software
description: En este tema se proporciona información sobre cómo planear la implementación de la infraestructura de red definida por software (SDN).
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
ms.openlocfilehash: e2c125867b461cee9f694849db5c8f61be91211d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869945"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planeación de una infraestructura de red definida por software

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Obtenga información sobre la planeación de la implementación de una infraestructura de red definida por software, incluidos los requisitos previos de hardware y software. 


## <a name="prerequisites"></a>Requisitos previos
En este tema se describe una serie de requisitos previos de hardware y software, entre los que se incluyen:

-   **Grupos de seguridad configurados, ubicaciones de archivos de registro y registro de DNS dinámico** Debe preparar su centro de información para la implementación de la controladora de red, que requiere uno o varios equipos o máquinas virtuales, y un equipo o máquina virtual. Para poder implementar la controladora de red, debe configurar los grupos de seguridad, las ubicaciones de los archivos de registro (si es necesario) y el registro de DNS dinámico.  Si no ha preparado el centro de datos para la implementación de la controladora de red, vea [requisitos de instalación y preparación para](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) la implementación de la controladora de red para más información.

-   **Red física**  Necesita acceso a los dispositivos de red físicos para configurar redes VLAN, enrutamiento, BGP, protocolo de puente del centro de datos (ETS) si usa una tecnología RDMA y el protocolo de puente del centro de datos (PFC) si usa una tecnología RDMA basada en RoCE. En este tema se muestra la configuración del conmutador manual, así como el emparejamiento BGP en conmutadores y enrutadores de nivel 3, o en una máquina virtual del servidor de enrutamiento y acceso remoto (RRAS).   

-   **Hosts de proceso físicos**  Estos hosts ejecutan Hyper-V y son necesarios para hospedar la infraestructura de SDN y las máquinas virtuales de inquilino.  Se necesita hardware de red específico en estos hosts para obtener el mejor rendimiento, que se describe más adelante en la sección **hardware de red** .  


## <a name="physical-network-and-compute-host-configuration"></a>Configuración de la red física y del host de proceso

Cada host de proceso físico requiere conectividad de red a través de uno o varios adaptadores de red conectados a un puerto de conmutador físico.  Una [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN) de nivel 2 admite redes divididas en varios segmentos de red lógica. 

>[!TIP]
>Usar VLAN 0 para redes lógicas en modo de acceso o sin etiquetar. 

>[!IMPORTANT]
>Las redes definidas por software de Windows Server 2016 admiten el direccionamiento IPv4 para proporcionaban y la superposición. No se admite IPv6.

### <a name="logical-networks"></a>Redes lógicas

#### <a name="management-and-hnv-provider"></a>Proveedor de administración y HNV 

Todos los hosts de proceso físicos deben tener acceso a la red lógica de administración y a la red lógica del proveedor de HNV.  En lo que respecta a la planeación de direcciones IP, cada host de proceso físico debe tener al menos una dirección IP asignada desde la red lógica de administración. La controladora de red requiere una dirección IP reservada para servir como dirección IP de REST. 

Un servidor DHCP puede asignar automáticamente direcciones IP para la red de administración o puede asignar manualmente una dirección IP estática. La pila de SDN asigna automáticamente direcciones IP para la red lógica del proveedor de HNV para los hosts de Hyper-V individuales de un grupo de direcciones IP especificado a través de y administrados por la controladora de red. 

>[!NOTE]
>La controladora de red asigna una dirección IP del proveedor de HNV a un host de proceso físico solo después de que el agente de host de la controladora de red reciba la Directiva de red para una máquina virtual de inquilino específica. 


|                                                               Si...                                                               |                                                                                                                                                                          En ese caso...                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                  Las redes lógicas usan VLAN,                                                  |                                                                 el host de proceso físico debe conectarse a un puerto de conmutador troncal que tenga acceso a estas VLAN. Es importante tener en cuenta que los adaptadores de red físicos del host del equipo no deben tener activado ningún filtrado de VLAN.                                                                 |
|                Usar la formación de equipos incrustada (establecida) y tener varios miembros del equipo NIC, como adaptadores de red,                |                                                                                                                        debe conectar todos los miembros del equipo NIC para ese host determinado al mismo dominio de difusión de nivel 2.                                                                                                                         |
| El host de proceso físico ejecuta máquinas virtuales de infraestructura adicionales, como controladora de red, SLB/MUX o puerta de enlace. | dicho host debe tener una dirección IP adicional asignada desde la red lógica de administración para cada una de las máquinas virtuales hospedadas.<p>Además, cada máquina virtual de infraestructura de SLB/MUX debe tener una dirección IP reservada para la red lógica del proveedor de HNV. Si no se puede tener una dirección IP reservada, se pueden producir direcciones IP duplicadas en la red. |

---

Para obtener información acerca de la virtualización de red de Hyper-V (HNV), que puede usar para virtualizar redes en una implementación de Microsoft SDN, consulte [virtualización de red de Hyper-v](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  



#### <a name="gateways-and-the-software-load-balancer"></a>Puertas de enlace y el software Load Balancer

Es necesario crear y aprovisionar redes lógicas adicionales para el uso de la puerta de enlace y SLB. Asegúrese de obtener los prefijos IP correctos, los identificadores de VLAN y las direcciones IP de puerta de enlace para estas redes.


|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **Red lógica de tránsito**   | La puerta de enlace de RAS y SLB/MUX usan la red lógica de tránsito para intercambiar información de emparejamiento de BGP y el tráfico de inquilinos de norte/sur (interno externo). Normalmente, el tamaño de esta subred será menor que el de los demás. Solo los hosts de proceso físico que ejecutan máquinas virtuales de puerta de enlace RAS o SLB/MUX deben tener conectividad a esta subred con estas VLAN troncales y accesibles en los puertos de conmutador a los que están conectados los adaptadores de red de los hosts de proceso. Cada máquina virtual de SLB/MUX o de puerta de enlace RAS está asignada estáticamente a una dirección IP de la red lógica de tránsito. |
| **Red lógica de VIP pública**  |                                                                                                                             La red lógica de VIP pública debe tener prefijos de subred IP que sean enrutables fuera del entorno de nube (normalmente enrutable a Internet).  Serán las direcciones IP de front-end que usan los clientes externos para tener acceso a los recursos de las redes virtuales, incluida la VIP de front-end para la puerta de enlace de sitio a sitio.                                                                                                                             |
| **Red lógica de VIP privada** |                                                                                                                                                                                       No es necesario que la red lógica de VIP privada sea enrutable fuera de la nube, ya que se usa para VIP a las que solo se tiene acceso desde clientes internos de la nube, como el uso de SLB o servicios privados.                                                                                                                                                                                       |
|   **Red lógica VIP GRE**   |                                                                                                                                           La red VIP GRE es una subred que existe únicamente para definir VIP que se asignan a máquinas virtuales de puerta de enlace que se ejecutan en el tejido de SDN para un tipo de conexión GRE S2S. No es necesario configurar esta red previamente en los conmutadores físicos o en el enrutador y no es necesario que tenga una VLAN asignada.                                                                                                                                            |

---


#### <a name="sample-network-topology"></a>Topología de red de ejemplo
Cambie los prefijos de subred IP de ejemplo y los identificadores de VLAN para su entorno. 


| **Nombre de red** |  **Subred**  | **Máscara** | **IDENTIFICADOR de VLAN en camión** | **Puerta**  |                                                           **Reservas (ejemplos)**                                                           |
|------------------|--------------|----------|----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|    Administración    | 10.184.108.0 |    24    |          7           | 10.184.108.1 | 10.184.108.1 – router 10.184.108.4-10.184.108.10 de la controladora de red-Compute host 110.184.108.11-Compute host 210.184.108. X-Compute host X |
|   Proveedor HNV   |  10.10.56.0  |    23    |          11          |  10.10.56.1  |                                                    10.10.56.1: enrutador 10.10.56.2-SLB/MUX1                                                     |
|     Tránsito      |  red 10.10.10.0  |    24    |          10          |  10.10.10.1  |                                                               10.10.10.1: enrutador                                                               |
|    VIP pública    |  41.40.40.0  |    27    |          N/D          |  41.40.40.1  |                                    41.40.40.1 – router 41.40.40.2-SLB/MUX VIP 41.40.40.3-IPSec S2S VPN VIP                                    |
|   VIP privada    |  20.20.20.0  |    27    |          N/D          |  20.20.20.1  |                                                        20.20.20.1: GW predeterminado (enrutador)                                                         |
|     VIP GRE      |  31.30.30.0  |    24    |          N/D          |  31.30.30.1  |                                                             31.30.30.1: GW predeterminado                                                             |

---

### <a name="logical-networks-required-for-rdma-based-storage"></a>Redes lógicas requeridas para el almacenamiento basado en RDMA  

Si usa el almacenamiento basado en RDMA, defina una VLAN y una subred para cada adaptador físico (dos adaptadores por nodo) en los hosts de proceso y almacenamiento.  

>[!IMPORTANT]
>Para que la calidad de servicio (QoS) se aplique correctamente, los conmutadores físicos requieren una VLAN etiquetada para el tráfico RDMA.

| **Nombre de red** |  **Subred**  | **Máscara** | **IDENTIFICADOR de VLAN en camión** | **Puerta**  |                                                           **Reservas (ejemplos)**                                                            |
|------------------|--------------|----------|----------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
|     storage1     |  10.60.36.0  |    25    |          8           |  10.60.36.1  |  10.60.36.1: enrutador<p>10.60.36. x: host de proceso X<p>10.60.36. y-compute host Y<p>10.60.36. V: clúster de proceso<p>10.60.36. W: clúster de almacenamiento  |
|     storage2     | 10.60.36.128 |    25    |          9           | 10.60.36.129 | 10.60.36.129: enrutador<p>10.60.36. x: host de proceso X<p>10.60.36. y-compute host Y<p>10.60.36. V: clúster de proceso<p>10.60.36. W: clúster de almacenamiento |

---


## <a name="routing-infrastructure"></a>Infraestructura de enrutamiento  

Si va a implementar la infraestructura de SDN mediante scripts, las subredes administración, proveedor de HNV, tránsito y VIP deben ser enrutables entre sí en la red física.     

La información \(de enrutamiento, por ejemplo\) , el próximo salto para las subredes VIP se anuncia por las puertas de enlace de SLB/MUX y ras en la red física mediante el emparejamiento BGP interno. Las redes lógicas de VIP no tienen una VLAN asignada y no está preconfigurada en el conmutador de capa 2 (por ejemplo, conmutador para parte superior del rack).  

Debe crear un par BGP en el enrutador que usa la infraestructura de SDN para recibir rutas para las redes lógicas de VIP anunciadas por las puertas de enlace de SLB/MUX y RAS. El emparejamiento BGP solo tiene que producirse de una manera (desde el SLB/MUX o la puerta de enlace de RAS al par BGP externo).  Por encima de la primera capa de enrutamiento puede utilizar rutas estáticas u otro protocolo de enrutamiento dinámico como OSPF; sin embargo, como se indicó anteriormente, el prefijo de subred IP para las redes lógicas VIP debe ser enrutable desde la red física al par BGP externo.   

El emparejamiento BGP normalmente se configura en un conmutador o enrutador administrado como parte de la infraestructura de red. El par BGP también puede configurarse en un servidor Windows con el rol de servidor de acceso remoto (RAS) instalado en un modo de solo enrutamiento. Este enrutador BGP del mismo nivel en la infraestructura de red debe configurarse para que tenga su propio ASN y permitir el emparejamiento de un ASN \(que esté asignado a los componentes de\)Sdn SLB/MUX y las puertas de enlace de Ras. Debe obtener la siguiente información del enrutador físico o del administrador de red en el control del enrutador:

- ASN de enrutador  
- Dirección IP del enrutador  
- ASN para su uso por parte de los componentes de SDN (puede ser cualquier número como del intervalo privado de ASN)

>[!NOTE]
>El SLB/MUX no admiten ASN de cuatro bytes. Debe asignar dos ASN de byte a SLB/MUX y el enrutador WO al que se conecta. Puede usar ASN de 4 bytes en cualquier parte de su entorno.  

Usted o el administrador de red deben configurar el enrutador BGP del mismo nivel para que acepte conexiones de la dirección IP o de ASN, o la dirección de subred de la red lógica de tránsito que estén usando la puerta de enlace de RAS y SLB/Mux.

Para obtener más información, vea [Protocolo de puerta de enlace de borde (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

## <a name="default-gateways"></a>Puertas de enlace predeterminadas
Las máquinas que están configuradas para conectarse a varias redes, como los hosts físicos y las máquinas virtuales de puerta de enlace, solo deben tener una puerta de enlace predeterminada configurada. Configurar la puerta de enlace predeterminada en el adaptador que se usa para tener acceso a Internet.

En el caso de las máquinas virtuales, siga estas reglas para decidir qué red usar como puerta de enlace predeterminada:

1. Use la red lógica de tránsito como la puerta de enlace predeterminada si una máquina virtual se conecta a la red de tránsito, o si es de host múltiple a la red de tránsito o a cualquier otra red.
2. Use la red de administración como puerta de enlace predeterminada si una máquina virtual solo se conecta a la red de administración. 
3. Use la red del proveedor de HNV para las puertas de enlace de SLB/MUX y RAS. No use la red del proveedor de HNV como puerta de enlace predeterminada. 
4. No conecte las máquinas virtuales directamente a las redes VIP privadas o de Storage1, Storage2 o VIP públicas.

En el caso de los hosts de Hyper-V y los nodos de almacenamiento, use la red de administración como la puerta de enlace predeterminada.  Las redes de almacenamiento nunca deben tener asignada una puerta de enlace predeterminada.


## <a name="network-hardware"></a>Hardware de red

Puede usar las siguientes secciones para planear la implementación de hardware de red.

### <a name="network-interface-cards-nics"></a>Tarjetas de interfaz de red (NIC)

Las tarjetas de interfaz de red (NIC) usadas en los hosts de Hyper-V y en los hosts de almacenamiento requieren capacidades específicas para lograr el mejor rendimiento. 

El acceso directo a memoria remota (RDMA) es una técnica de omisión de kernel que permite transferir grandes cantidades de datos sin usar la CPU del host, lo que libera la CPU para realizar otro trabajo. 

Switch Embedded Teaming (SET) es una solución alternativa de formación de equipos NIC que puede usar en entornos que incluyen Hyper-V y la pila de redes definidas por software (SDN) en Windows Server 2016. El conjunto integra la funcionalidad de formación de equipos NIC en el conmutador virtual de Hyper-V. 

Para obtener más información, vea [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).   

Para tener en cuenta la sobrecarga en el tráfico de red virtual de inquilinos causada por los encabezados de encapsulación VXLAN o NVGRE, la MTU de la red de tejido de capa 2 (conmutadores y hosts) debe establecerse \(en un valor mayor o igual que 1674 bytes, como Ethernet de capa 2. \)encabezados. 

Las NIC que admiten la nueva palabra clave de adaptador avanzado *EncapOverhead* establecen la MTU automáticamente a través del agente de host de la controladora de red. Las NIC que no admiten la palabra clave New *EncapOverhead* deben establecer el tamaño de MTU manualmente en cada host físico mediante la palabra\) clave *JumboPacket* \(o equivalente. 


### <a name="switches"></a>Centrales

Al seleccionar un conmutador físico y un enrutador para su entorno, asegúrese de que admite el siguiente conjunto de capacidades:  

- Configuración \(de MTU de switchport requerida\)  
- MTU establecida en > = 1674 bytes \(, incluido el encabezado L2-Ethernet\)  
- Protocolos \(L3 necesarios\)  
- ECMP  
- ECMP \(basado en BGP\)IETF RFC 4271\-

Las implementaciones deben admitir las instrucciones debe en los estándares IETF siguientes.

- RFC 2545: "Extensiones multiprotocolo de BGP-4 para el enrutamiento entre dominios IPv6"  
- RFC 4760: "Extensiones multiprotocolo para BGP-4"  
- RFC 4893: "Compatibilidad con BGP para cuatro octetos como espacio de número"  
- RFC 4456: "Reflexión de rutas BGP: Una alternativa a BGP interno de la malla completa (IBGP)  
- RFC 4724: "Mecanismo de reinicio estable para BGP"  

Se requieren los siguientes protocolos de etiquetado.

- VLAN: aislamiento de varios tipos de tráfico
- tronco 802.1 q

Los elementos siguientes proporcionan el control de vínculos.

- Solo se requiere \(PFC de calidad de servicio si se usa RoCE\)
- Selección \(de tráfico mejorada 802.1 QAZ\)
- Control \(de flujo basado en prioridades 802.1 p/Q y 802.1 QBB\)

Los elementos siguientes proporcionan disponibilidad y redundancia.

- Disponibilidad del conmutador (obligatorio)
- Se requiere un enrutador de alta disponibilidad para realizar funciones de puerta de enlace. Para ello, puede usar un conmutador de varios chasis o tecnologías como VRRP.

Los elementos siguientes proporcionan capacidades de administración.

**Evaluación**

- SNMP V1 o SNMP V2 (obligatorio si se usa la controladora de red para la supervisión de conmutadores físicos)  
- MIB \(de SNMP necesarios si usa la controladora de red para la supervisión de conmutadores físicos\)  
- MIB-II (RFC 1213), LLDP, interfaz MIB \(RFC 2863\), if-MIB, IP-MIB, IP-Forward-MIB, Q-Bridge-MIB, Bridge-MIB, LLDB-MIB, Entity-MIB, IEEE8023-lag-MIB  

En los diagramas siguientes se muestra un ejemplo de configuración de cuatro nodos. Por motivos de claridad, el primer diagrama muestra solo el controlador de red, el segundo muestra el controlador de red más el equilibrador de carga de software y el tercer diagrama muestra la controladora de red, el equilibrador de carga de software y la puerta de enlace.  

En estos diagramas no se muestran las redes de almacenamiento ni VNIC. Si tiene previsto usar el almacenamiento basado en SMB, es necesario.

Tanto la infraestructura como las máquinas virtuales de inquilino se pueden redistribuir en cualquier host de proceso físico (siempre que exista la conectividad de red correcta para las redes lógicas correctas).  



## <a name="switch-configuration-examples"></a>Cambiar ejemplos de configuración  

Para ayudar a configurar el conmutador físico o enrutador, hay disponible un conjunto de archivos de configuración de ejemplo para una variedad de proveedores y modelos de conmutadores en el [repositorio de github de SDN de Microsoft](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Se proporciona un archivo Léame detallado y comandos de la interfaz de la línea de comandos (CLI) probados para conmutadores específicos.         


## <a name="compute"></a>Proceso  
Todos los hosts de Hyper-V deben tener instalado Windows Server 2016, Hyper-V habilitado y un conmutador virtual externo de Hyper-V creado con al menos un adaptador físico conectado a la red lógica de administración. El host debe ser accesible a través de una dirección IP de administración asignada al host de administración vNIC.  

Se puede usar cualquier tipo de almacenamiento que sea compatible con Hyper-V, compartido o local.   

> [!TIP]  
> Es conveniente usar el mismo nombre para todos los conmutadores virtuales, pero no es obligatorio. Si planea implementar con scripts, vea el comentario asociado a la `vSwitchName` variable en el archivo config. psd1.  

**Requisitos de proceso del host**  
En la tabla siguiente se muestran los requisitos mínimos de hardware y software para los cuatro hosts físicos usados en la implementación de ejemplo.  

Host|Requisitos de hardware|Requisitos de software|  
--------|-------------------------|-------------------------  
|Host de Hyper-v físico|CPU de 4 núcleos a 2,66 GHz<br /><br />32 GB de RAM<br /><br />300 GB de espacio en disco<br /><br />adaptador de red físico de 1 GB/s (o superior)|OPERATIVOS Windows Server 2016<br /><br />Rol de Hyper-V instalado|  


**Requisitos de rol de máquina virtual de infraestructura de SDN**  

Rol|requisitos de vCPU|Requisitos de memoria|Requisitos de disco|  
--------|---------------------|-----------------------|---------------------  
|Controladora de red (tres nodos)|4 vCPU|4 GB como mínimo (se recomiendan 8 GB)|75 GB para la unidad del sistema operativo  
|SLB/MUX (tres nodos)|8 vCPU|se recomiendan 8 GB|75 GB para la unidad del sistema operativo  
|Puerta de enlace RAS<br /><br />(grupo único de las puertas de enlace de tres nodos, dos activas, una pasiva)|8 vCPU|se recomiendan 8 GB|75 GB para la unidad del sistema operativo  
|Enrutador BGP de puerta de enlace RAS para emparejamiento de SLB/MUX<br /><br />(también puede usar el modificador ToR como enrutador BGP)|2 vCPU|2 GB|75 GB para la unidad del sistema operativo|  


Si usa VMM para la implementación, se necesitan recursos de máquina virtual de infraestructura adicionales para VMM y otras infraestructuras que no son de SDN. Para obtener más información, consulte [recomendaciones mínimas de hardware para System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  

## <a name="extending-your-infrastructure"></a>Ampliación de la infraestructura  
Los requisitos de tamaño y recursos de la infraestructura dependen de las máquinas virtuales de carga de trabajo de inquilinos que planea hospedar. Los requisitos de CPU, memoria y disco para las máquinas virtuales de infraestructura (por ejemplo: controladora de red, SLB, puerta de enlace, etc.) se enumeran en la tabla anterior. Puede agregar más de estas máquinas virtuales de infraestructura para escalar horizontalmente según sea necesario. Sin embargo, las máquinas virtuales de inquilino que se ejecutan en los hosts de Hyper-V tienen sus propios requisitos de CPU, memoria y disco que se deben tener en cuenta.   

Cuando las máquinas virtuales de carga de trabajo de inquilinos empiecen a consumir demasiados recursos en los hosts de Hyper-V físicos, puede ampliar la infraestructura mediante la adición de hosts físicos adicionales. Esto se puede hacer con Virtual Machine Manager o mediante scripts de PowerShell (en función de cómo se implementó inicialmente la infraestructura) para crear nuevos recursos de servidor a través de la controladora de red. Si necesita agregar más direcciones IP para la red del proveedor de HNV, puede crear nuevas subredes lógicas (con los grupos de direcciones IP correspondientes) que los hosts pueden usar.  


## <a name="see-also"></a>Vea también  
[Requisitos de instalación y preparación para la implementación de la controladora de red](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Redes &#40;definidas por software Sdn&#41;](../Software-Defined-Networking--SDN-.md)  



