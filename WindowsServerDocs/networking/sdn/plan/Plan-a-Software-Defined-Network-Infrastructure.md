---
title: Planear una infraestructura de red definido de Software
description: Este tema proporciona información sobre cómo planear la implementación de infraestructura de red de definido de Software (SDN).
manager: brianlic
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
ms.openlocfilehash: bb3b9313996637fa5ee7367c538fe04d7cbefea9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planear una infraestructura de red definido de Software

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Revisa la siguiente información para ayudar a planear la implementación de infraestructura de red de definido de Software (SDN). Después de revisar esta información, consulta el tema [implementar una infraestructura de red de Software definido](../deploy/Deploy-a-Software-Defined-Network-Infrastructure.md) para obtener información de implementación.

>[!NOTE]
>Además de este tema, el siguiente contenido planeación SDN está disponible.  
>
> - [Instalación y requisitos de preparación para implementar el controlador de red](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  

Para obtener información sobre la virtualización de red de Hyper-V (HNV), que puedes usar la virtualización de redes en una implementación de Microsoft SDN, vea [virtualización de Hyper-V red](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  

## <a name="prerequisites"></a>Requisitos previos
Este tema describe una serie de requisitos previos de hardware y software, incluidos:

-   **Física de red**  
    Necesita tener acceso a los dispositivos de red física para configurar VLAN, enrutamiento, BGP, puente de centro de datos (NET) si usa una tecnología RDMA y datos centro puente (PFC) si usando un RoCE en función de la tecnología RDMA. Este tema muestra la configuración del conmutador manual como BGP interconexión en conmutadores de capa 3 o enrutadores o una máquina virtual de enrutamiento y el servidor de acceso remoto (RRAS).   

-   **Hosts de cálculo física**  
Estos hosts ejecutan Hyper-V y son necesarios para hospedar SDN infraestructura e inquilino las máquinas virtuales.  Se requiere hardware de red específicas en estos hosts para un mejor rendimiento, que se describe más adelante en el **hardware de red** sección.  
      
  
## <a name="physical-network-configuration"></a>Configuración de red física

Cada host físico cálculo requiere conectividad de red a través de uno o varios adaptadores de red asociados a los puertos de un conmutador físico. La red se divide en varios segmentos de red lógica, opcionalmente, respaldadas por una capa 2 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN). Los prefijos de subred IP y los identificadores de VLAN se muestra a continuación son ejemplos y deben ser personalizados para el entorno en función de instrucciones al administrador de red. Si cualquiera de las redes lógicas se etiquetan o en modo de acceso, usa el ID de VLAN 0 de estas redes al configurar las subredes lógicas en los archivos de configuración de script de System Center Virtual Machine Manager o PowerShell.

>[!IMPORTANT]
>Windows Server 2016 Software definido redes admite direcciones IPv4 para la subposición y la superposición. No se admite IPv6.
  
### <a name="management-and-hnv-provider-logical-networks"></a>Administración y el proveedor de HNV redes lógicas

Física todos calcular hosts deben tener acceso a la red lógica de administración y la red lógica HNV proveedor. Si las redes lógicas usan VLAN, los hosts de cálculo física deben estar conectado a un puerto del conmutador troncal que tiene acceso a estos VLAN. Del mismo modo, los adaptadores de red físico en el host de cálculo no deben tener ningún filtro de VLAN activado. Si usas Switch-Embedded agrupación (conjunto) y tienen varias NIC los miembros del equipo (es decir, adaptadores de red) en los hosts de cálculo, deberá conectarse todos los miembros del equipo NIC para ese host en particular para el mismo dominio de difusión de nivel 2.  
  
Para fines de planeación la dirección IP, cada host físico cálculo debe tener al menos una dirección IP que se asignan desde la red lógica de administración. El controlador de red asigna automáticamente exactamente dos direcciones IP de la red lógica HNV proveedor. Si el host físico cálculo ejecuta máquinas virtuales de infraestructura adicional (por ejemplo, el controlador de red, SLB/MUX o puerta de enlace) que el host debe tener una dirección IP adicional asignada desde la red lógica de administración para cada una de las máquinas virtuales de infraestructura hospedadas.   
  
Además, cada máquina virtual de infraestructura SLB/MUX debe tener una dirección IP reservada desde la red lógica HNV proveedor. 

>[!IMPORTANT]
>Estas direcciones IP SLB/MUX deben asignarse desde fuera del grupo de dirección IP que está configurado para la red lógica HNV proveedor. Si no haces esto puede generar direcciones IP duplicadas en la red. 

El controlador de red requiere una dirección reservada desde la red de administración para que actúe como la dirección IP del resto. Debe crear manualmente el registro de HOST en DNS para la dirección IP del resto.  
  
Un servidor DHCP puede asignar automáticamente direcciones IP para la red de administración o dirección IP estática puede asignar manualmente. La pila SDN asigna automáticamente direcciones IP para la red de proveedor HNV para los hosts de Hyper-V individuales de un grupo de IP especifican a través de y que administrados por el controlador de red.   
  
El Administrador de fabric estáticamente asigna las direcciones IP de proveedor HNV usadas por el SLB/MUX a través de scripts de PowerShell o VMM. El controlador de red asigna una dirección IP del proveedor de HNV a un host físico cálculo solo después de que el agente de Host del controlador de red recibe la directiva de red para una máquina virtual de inquilino específico.  
  
#### <a name="sample-network-topology"></a>Topología de red de ejemplo

Personalizar los prefijos de subred, VLAN identificadores y direcciones IP de puerta de enlace basadas en la Guía del Administrador de la red.  
  
Nombre de red|Subred|Máscara|Id. de VLAN en tronco|Puerta de enlace|Reservas<br />(ejemplos)  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**Administración**|10.184.108.0|24|7|10.184.108.1|10.184.108.1 - enrutador<br /><br />10.184.108.4 - controlador de red<br /><br />10.184.108.10 - cálculo host 1<br /><br />10.184.108.11 - cálculo host 2<br /><br />10.184.108.X - cálculo host X  
|**Proveedor HNV**|10.10.56.0|23|11|10.10.56.1|10.10.56.1 - enrutador<br /><br />10.10.56.2 - SLB/MUX1  
  
### <a name="logical-networks-for-gateways-and-the-software-load-balancer"></a>Redes lógicas para puertas de enlace y el equilibrado de carga de Software
  
Redes lógicas adicionales que necesite creado y aprovisionado para la puerta de enlace y el uso SLB. Una vez más, debes trabajar con el Administrador de red para obtener los prefijos IP correctos, VLAN identificadores y direcciones IP de puerta de enlace de estas redes.

#### <a name="transit-logical-network"></a>Red lógica de transporte público
  
La puerta de enlace de RAS y SLB/MUX usan la red lógica de tránsito para intercambiar información interconexión de BGP y el tráfico de inquilinos (externo interno) norte/sur. El tamaño de la subred suele ser más pequeño que otros. Solo los hosts de cálculo física que se ejecutan RAS puerta de enlace o máquinas virtuales SLB/MUX deben tener conectividad con esta subred con estos VLAN troncal y accesibles en los puertos del conmutador al que están conectados los adaptadores de red de los hosts de cálculo. Cada SLB/MUX o máquina virtual de puerta de enlace de RAS estáticamente se asigna una dirección IP de la red durante el tránsito.

#### <a name="public-vip-logical-network"></a>Red lógica VIP pública  
  
La red lógica VIP públicas deben ser los prefijos de subred IP que se pueda enrutables fuera del entorno de la nube (normalmente Internet pueden enrutar).  Se trata de las direcciones IP front-end que usan los clientes externos para acceder a recursos en las redes virtuales, incluido el front-end VIP para la puerta de enlace de sitio a sitio.

#### <a name="private-vip-logical-network"></a>Red privada de lógica de VIP
  
La red privada VIP no es necesaria que se pueden enrutar fuera de la nube que se usan para VIPs que solo se puede acceder desde clientes interno de nube, como el Administrador de SLB o servicios privada.
  
#### <a name="gre-vip-logical-network"></a>Red lógica GRE VIP

La red GRE VIP es una máscara de subred que existe solamente para definir VIPs que se asignan a las máquinas virtuales de puerta de enlace ejecutándose en su tejido SDN para un tipo de conexión S2S GRE. Esta red no es necesario configurar previamente en el modificadores físicas o el enrutador y que no tienen una VLAN asignada.   

### <a name="sample-network-topology"></a>Topología de red de ejemplo

Personalizar los prefijos de subred, VLAN identificadores y direcciones IP de puerta de enlace basadas en la Guía del Administrador de la red.  
  
Nombre de red|Subred|Máscara|Id. de VLAN en tronco|Puerta de enlace|Reservas<br />(ejemplos)  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**Transporte público**|10.10.10.0|24|10|10.10.10.1|10.10.10.1 - enrutador  
|**VIP pública**|41.40.40.0|27|NA|41.40.40.1|41.40.40.1 - enrutador<br /> 41.40.40.2 - SLB/MUX VIP<br />41.40.40.3 - IPSec S2S VPN VIP  
|**VIP privada**|20.20.20.0|27|NA|20.20.20.1|20.20.20.1 - GW predeterminado (enrutador)  
|**GRE VIP**|31.30.30.0|24|NA|31.30.30.1|31.30.30.1 - GW predeterminado|  
  
### <a name="logical-networks-required-for-rdma-based-storage"></a>Redes lógicas necesarias para el almacenamiento de archivos basado en RDMA  
  
Si estás usando RDMA almacenamiento basado en, tendrás que definir una VLAN y subred para cada adaptador físico en los hosts de cálculo y almacenamiento. Por lo general, tendrás dos adaptadores físicos por nodo para esta configuración.  
  
> [!IMPORTANT]  
> Más físicas conmutadores requieren tráfico RDMA enviarán en una VLAN etiquetada con el fin de calidad de la configuración del servicio se aplique correctamente.  No coloques el tráfico RDMA hacia una VLAN sin etiquetar o en un puerto de modo de acceso física.  
  
Nombre de red  |Subred  |Máscara  |Id. de VLAN en tronco  |Puerta de enlace  |Reservas<br />(ejemplos)    
---------|---------|---------|---------|---------|---------  
**: Storage1**     |    10.60.36.0     | 25        |   8      |  10.60.36.1       |  10.60.36.1 - enrutador<br />10.60.36.x - cálculo host x<br />10.60.36.y - y del host de cálculo<br />10.60.36.v - clúster de cálculo<br />10.60.36.w - clúster de almacenamiento  
|**: Storage2**|10.60.36.128|25|9|10.60.36.129|10.60.36.129 - enrutador<br />10.60.36.x - cálculo host x<br />10.60.36.y - y del host de cálculo<br />10.60.36.v - clúster de cálculo<br />10.60.36.w - clúster de almacenamiento  
  
Para obtener más información sobre la configuración de modificadores, consulta el **ejemplos de configuración** sección.  
  
## <a name="routing-infrastructure"></a>Infraestructura de enrutamiento  
  
Si vas a implementar la infraestructura SDN mediante scripts, las subredes de administración, HNV proveedor, transporte público y VIP deben ser pueden enrutables entre sí en la red física.     
  
Información de enrutamiento \ (por ejemplo, hop\ siguiente) de la dirección VIP subredes se anuncia el SLB/MUX y RAS puertas de enlace en la red física con interconexión de BGP interno. Las redes lógicas VIP no tienen una VLAN asignada y no está configuradas previamente en el conmutador de nivel 2 (por ejemplo, un conmutador parte superior de bastidor).  
  
Deberás crear un sistema del mismo nivel BGP en el enrutador de la infraestructura SDN sirve para recibir las rutas para las redes lógicas VIP ha anunciado por el SLB/MUXes y RAS puertas de enlace. Interconexión BGP solo debe realizarse una forma (de SLB/MUX o RAS puerta de enlace de un sistema del mismo nivel BGP externo).  Por encima de la primera capa de enrutamiento puede usar rutas estáticas o con otro protocolo de enrutamiento dinámico como OSPF, sin embargo, como se mencionó anteriormente, el prefijo de subred IP para las redes lógicas VIP es necesario de la red pueden enrutar el sistema del mismo nivel BGP externo.   
  
Interconexión BGP normalmente se configura en un conmutador o enrutador gestionado como parte de la infraestructura de red. El sistema del mismo nivel BGP también puede configurarse en un servidor de Windows con el rol de servidor de acceso remoto (RAS) instalado en un modo de enrutamiento solo. Este nivel de enrutador BGP en la infraestructura de red debe estar configurado para tener su propio ASN y permitir el análisis detallado de un ASN que se asigna a los componentes SDN \ (SLB/MUX y Gateways\ RAS). Desde el enrutador físico, o desde el Administrador de red en un control de enrutador debe obtener la información siguiente:

- Enrutador ASN  
- Dirección IP del enrutador  
- ASN para su uso por componentes SDN (puede ser cualquier número como del intervalo ASN privado)

>[!NOTE]
>La Multiplexación/SLB no admiten ASNs de cuatro bytes. Debe asignar dos bytes ASNs el MUX de SLB y el enrutador wo que se conecta. Puedes usar 4 bytes ASNs en otro lugar en tu entorno.  
  
Usted o el Administrador de red debe configurar el sistema del mismo nivel de enrutador BGP para aceptar conexiones desde el ASN y la dirección IP o la dirección de subred de la red durante el tránsito lógica que usan la puerta de enlace RAS y SLB/MUXes.
  
Para obtener más información, consulta [protocolo de enlace de borde & #40; BGP & #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).
  
## <a name="default-gateways"></a>Puertas de enlace predeterminada

Los equipos que están configurados para conectar con varias redes, como los hosts físicos y máquinas virtuales de puerta de enlace deben tener solo una puerta de enlace predeterminada configurado. Puerta de enlace predeterminada normalmente se configurará en el adaptador utilizado para llegar a llegar a Internet.

Para las máquinas virtuales, usa las siguientes reglas para decidir a qué red para utilizarlos como la puerta de enlace predeterminada:

1. Si una máquina virtual está conectada a la red de tránsito, o si es multi-alojados en tránsito y cualquier otra red, usa la red de tránsito como la puerta de enlace predeterminada.  
2. Usa la red de administración como la puerta de enlace predeterminada si una máquina virtual solo está conectada a la red de administración.  
3.  La red de proveedor HNV nunca debe usarse como una puerta de enlace predeterminada. Las máquinas virtuales solo conectadas a esta red será el SLB/MUXes y RAS puertas de enlace.  
4.  Nunca se conectará directamente a las redes: Storage1,: Storage2, VIP pública o privada VIP máquinas virtuales.  
  
Para los hosts de Hyper-V y nodos de almacenamiento, usa la red de administración como la puerta de enlace predeterminada.  Las redes de almacenamiento nunca deben tener una puerta de enlace predeterminada asignado.
  
## <a name="network-hardware"></a>Hardware de red

Puedes usar las siguientes secciones para planear la implementación de hardware de red.

### <a name="network-interface-cards-nics"></a>Tarjetas de interfaz de red (NIC)

Para lograr un rendimiento óptimo, las capacidades específicas son necesarias en las tarjetas de interfaz de red que usas en tu hosts de Hyper-V y almacenamiento.  
 
Memoria de acceso directo remoto (RDMA) es un kernel omitir técnica que permite transferir grandes cantidades de datos sin necesidad de la CPU host. Dado que el motor de DMA en el adaptador de red realiza a la transferencia, la CPU no se usa para el movimiento de la memoria.  Esto libera la CPU para realizar otro trabajo.  

Modificador incrustado agrupación (conjunto de) es una alternativa solución equipo NIC que puedes usar en entornos que incluyen Hyper-V y la pila de Software definido de redes (SDN) en Windows Server 2016. CONJUNTO integra algunas funciones de equipo NIC en el conmutador de Hyper-V Virtual.

Para obtener más información, consulta [remoto la acceso directo a memoria & #40; RDMA & #41; y cambiar la agrupación incrustado & #40; Establecer & #41;](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

Para explicar la sobrecarga en el tráfico de red virtual de inquilino causado por VXLAN o NVGRE encabezados encapsulación, debe establecerse el MTU de la red de fabric de nivel 2 (modificadores y hosts) a mayor o igual a Bytes 1674 \ (incluidos headers\ Ethernet de nivel 2). NIC que admiten la nueva *EncapOverhead* palabra clave de adaptador avanzadas establecerá la MTU automáticamente mediante el agente de Host del controlador de red. NIC que no son compatibles con el nuevo *EncapOverhead* palabra clave que se necesita establecer el tamaño MTU manualmente en cada host físico con la *JumboPacket* palabra clave \(or equivalent\).

### <a name="switches"></a>Modificadores
  
Al seleccionar un conmutador físico y un enrutador para su entorno Asegúrese de que admite el siguiente conjunto de funcionalidades.  

- MTU Switchport configuración \(required\)  
- MTU se establece en > = Bytes 1674 \ (incluidos L2 Ethernet Header\)  
- L3 protocolos \(required\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-based ECMP

Implementaciones deben admitir las instrucciones de hacerlo en los siguientes estándares IETF.

- RFC 2545: "BGP 4 múltiples protocolos extensiones para el enrutamiento de interdominios IPv6"  
- RFC 4760: "extensiones múltiples protocolos para BGP 4"  
- RFC 4893: "BGP soporte para cuatro octet AS número espacio"  
- RFC 4456: "ruta BGP reflexión: una alternativa a completa malla BGP interno (IBGP)"  
- RFC 4724: "mecanismo reinicio normal para BGP"  

Los siguientes protocolos etiquetas son necesarios.

- VLAN: aislamiento de diversos tipos de tráfico
- 802.1Q tronco

Los elementos siguientes proporcionan control del vínculo.

- Calidad de servicio \ (PFC sólo es necesario si usando RoCE\)
- Mejorado el tráfico \(802.1Qaz\) selección
- Control de flujo basada en prioridades \ (802.1X p/Q y 802.1Qbb\)

Los siguientes elementos ofrecen disponibilidad y redundancia.

- Cambiar de disponibilidad (necesario)
- Un enrutador altamente disponible es necesaria para realizar funciones de puerta de enlace. Puede hacerlo mediante un enrutador chasis múltiples switch\ o tecnologías como VRRP.
        
Los siguientes elementos ofrecen funcionalidades de administración.

**Supervisión**

- SNMP v1 o v2 SNMP (necesario si se usa el controlador de red para supervisar el interruptor físico)  
- MIB SNMP \ (necesario si estás usando el controlador de red para monitoring\ interruptor físico)  
- Mibii (RFC 1213), LLDP, interfaz MIB de IP MIB \(RFC 2863\), IF-MIB, MIB de avance de IP, Q MIB de puente, puente MIB, LLDB MIB, entidad MIB, IEEE8023 MIB de retraso  
  
Los siguientes diagramas muestran una configuración del nodo cuatro de ejemplo. Por motivos de claridad, el primer diagrama muestra solo el controlador de red, la segunda muestra el controlador de red más el equilibrado de carga de software y el tercer diagrama muestra el controlador de red, equilibrado de carga de software y la puerta de enlace.  
  
VNICs y redes de almacenamiento no son shonwn en los diagramas siguientes. Si tienes previsto usar el almacenamiento de archivos basado en SMB, son necesarios.    
  
Máquinas virtuales de la infraestructura y el inquilino puede distribuirse a través de cualquier host de cálculo físico (suponiendo que existe la conectividad de red correcto para las redes lógicas correctas).  
  
### <a name="network-controller-deployment"></a>Implementación de controladores de red

Antes de implementar el controlador de red, debe revisar la instalación y requisitos de software, así como configurar grupos de seguridad y registro DNS dinámico. Para obtener más información, consulta [requisitos de preparación para implementar el controlador de red y la instalación](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md).

La configuración es altamente disponible con tres nodos de controlador de red configurados en máquinas virtuales. También se muestra es dos inquilinos con la red virtual del inquilino 2 dividido en dos subredes virtuales para simular un nivel de la web y un nivel de base de datos.  

![Planeación de SDN CN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Implementación de equilibrado de carga de software y controladores de red

Para una alta disponibilidad, hay dos o más nodos SLB/MUX.
   
![Planeación de SDN CN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Implementación de controlador de red, equilibrado de carga de Software y RAS puerta de enlace

Hay tres máquinas virtuales de puerta de enlace; dos están activas, y uno es redundante.

![Planeación de SDN CN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
Para la automatización de la implementación basada en TP5, Active Directory debe estar disponible y accesible desde estas subredes. Para obtener más información acerca de Active Directory, consulte [introducción de servicios de dominio de Active Directory](https://technet.microsoft.com/en-us/library/mt703721.aspx).  
  
>[!IMPORTANT] 
>Si implementas usar VMM, asegúrate de las máquinas virtuales de infraestructura (servidor VMM, AD/DNS, SQL Server, etc.) no se hospedan en cualquiera de los cuatro hosts que se muestra en los diagramas.  
  
## <a name="switch-configuration-examples"></a>Cambiar los ejemplos de configuración  
  
Para ayudar a configurar el interruptor físico o el enrutador, están disponibles en un conjunto de archivos de configuración de ejemplo para una variedad de modelos de conmutador y proveedores la [repositorio de Github de Microsoft SDN](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Se proporcionan un archivo Léame detallado y comandos de interfaz de línea de comandos (CLI) probado para modificadores específicos.         
  
  
## <a name="compute"></a>Calcular  
Todos los host de Hyper-V deben tener instalado Windows Server 2016, Hyper-V habilitado y un conmutador virtual de Hyper-V externo creado con al menos un adaptador físico conectado a la red lógica de administración. El host debe ser accesible a través de una dirección IP de administración asignada a la vNIC de Host de administración.  
  
Puede usar cualquier tipo de almacenamiento que sea compatible con Hyper-V, compartida o local.   
  
> [!TIP]  
> Es conveniente si usa el mismo nombre para todos los conmutadores virtuales, pero no es obligatorio. Si piensas implementar con scripts, consulta el comentario asociado a la `vSwitchName`variable en el archivo config.psd1.  
  
**Requisitos de cálculo de host**  
La siguiente tabla muestra los requisitos mínimos de hardware y software para los cuatro hosts físicos utilizados en la implementación de ejemplo.  
  
Host|Requisitos de hardware|Requisitos de software|  
--------|-------------------------|-------------------------  
|Host de Hyper-v físico|4 núcleos 2,66 GHz CPU<br /><br />32 GB de RAM<br /><br />Espacio en disco de 300 GB<br /><br />1 Gb/s (o superior) adaptador de red físico|El sistema operativo: Windows Server 2016<br /><br />Rol de Hyper-V instalado|  
  
  
**Requisitos de rol de máquina virtual de infraestructura de SDN**  
  
Función|Requisitos de vCPU|Requisitos de memoria|Requisitos de disco|  
--------|---------------------|-----------------------|---------------------  
|Controlador de red (nodo tres)|4 vCPUs|4 GB min (se recomiendan 8 GB)|75 GB para la unidad del sistema operativo  
|SLB/MUX (nodo tres)|8 vCPUs|Se recomienda 8 GB|75 GB para la unidad del sistema operativo  
|Puerta de enlace RAS<br /><br />(conjunto de tres puertas de enlace de nodo activa dos, uno pasivo)|8 vCPUs|Se recomienda 8 GB|75 GB para la unidad del sistema operativo  
|Enrutador RAS puerta de enlace BGP para SLB/MUX interconexión<br /><br />(o bien usa el modificador de términos de referencia como enrutador BGP)|2 vCPUs|2 GB|75 GB para la unidad del sistema operativo|  
  
  
Si usas VMM para la implementación, recursos de la máquina virtual de infraestructura adicional son necesarios para VMM y otras infraestructuras de no SDN. Para obtener más información, consulta [recomendaciones de Hardware mínima para System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  
  
## <a name="extending-your-infrastructure"></a>Ampliación de la infraestructura  
Los requisitos de tamaño y recursos para la infraestructura dependen de las máquinas virtuales de carga de trabajo de inquilino que Planeas hospedar. La CPU, memoria y requisitos de disco para las máquinas virtuales de infraestructura (por ejemplo: puerta de enlace de controlador, SLB, red, etc.) se enumeran en la tabla anterior. Puedes agregar más de estas máquinas virtuales de infraestructura escalar según sea necesario. Sin embargo, las máquinas virtuales de inquilino ejecutan en el host de Hyper-V tienen sus propias CPU, memoria y los requisitos de disco que debes tener en cuenta.   
  
Cuando las máquinas virtuales de carga de trabajo de inquilino empiezan a consumir demasiados recursos en los hosts de Hyper-V físicos, puedes ampliar la infraestructura agregando hosts físicos adicionales. Esto puede hacerse con el Administrador de máquina Virtual o mediante el uso de scripts de PowerShell (en función de cómo inicialmente se implementaba la infraestructura) para crear nuevos recursos del servidor a través del controlador de red. Si es necesario agregar más direcciones IP para la red HNV proveedor, puedes crear nuevas subredes lógicas (con grupos de IP correspondiente) que pueden utilizar los hosts.  
  
  
## <a name="see-also"></a>Consulta también  
[Instalación y requisitos de preparación para implementar el controlador de red](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Software definido Networking & #40; SDN & #41;](../Software-Defined-Networking--SDN-.md)  
  


