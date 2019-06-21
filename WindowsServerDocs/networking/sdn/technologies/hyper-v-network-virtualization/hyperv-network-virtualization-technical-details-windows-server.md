---
title: Detalles técnicos de virtualización de red de Hyper-V en Windows Server 2016
description: En este tema proporciona información técnica acerca de la virtualización de red de Hyper-V en Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b33c96ff7150b0dc4f6f93d2fa24741c9762a799
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282284"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Detalles técnicos de virtualización de red de Hyper-V en Windows Server 2016

>Se aplica a:  Windows Server 2016

La virtualización de servidores permite que se ejecuten varias instancias de servidor al mismo tiempo en un solo host físico; aunque las instancias del servidor estén aisladas entre sí. Cada máquina virtual básicamente funciona como si fuera el único servidor que se ejecuta en el equipo físico.  

Virtualización de red ofrece una funcionalidad similar, en qué varias redes virtuales (posiblemente con direcciones IP superpuestas) que se ejecutan en la misma infraestructura de red físico y cada red virtual funciona como si fuera la única red virtual que se ejecuta en la infraestructura de red compartida. La Figura 1 muestra esta relación.  

![Virtualización de servidor frente a virtualización de red](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  

Figura 1: Virtualización de servidor frente a virtualización de red  

## <a name="hyper-v-network-virtualization-concepts"></a>Conceptos de la virtualización de red de Hyper-V  
En virtualización de red de Hyper-V (HNV), un cliente o inquilino se define como el "propietario" de un conjunto de subredes IP que se implementan en una empresa o centro de datos. Un cliente puede ser una corporación o una empresa con varios departamentos o unidades de negocio en un centro de datos privado que requieran el aislamiento de red o un inquilino en un centro de datos público que es hospedado por un proveedor de servicios. Cada cliente puede tener uno o varios [redes virtuales](#VirtualNetworks) en el centro de datos y cada elemento virtual network se compone de uno o varios [subredes virtuales](#VirtualSubnets).  

Hay dos implementaciones de HNV que estarán disponibles en Windows Server 2016: HNVv1 y HNVv2.  

-   **HNVv1**  

    HNVv1 es compatible con Windows Server 2012 R2 y System Center 2012 R2 Virtual Machine Manager (VMM). Configuración de HNVv1 se basa en la administración de WMI y los cmdlets de Windows PowerShell (que facilita a través de System Center VMM) para definir la configuración de aislamiento y la dirección de cliente (CA): red virtual: las asignaciones de direcciones físicas (PA) y enrutamiento. No hay características adicionales se han agregado a HNVv1 en Windows Server 2016 y no se prevé ningún nuevas características.  

    • Establezca la formación de equipos y V1 de HNV no son compatibles con la plataforma.

    o usar los usuarios de las puertas de enlace de alta disponibilidad NVGRE no necesita a cualquiera use equipo LBFO o. O bien

    conmutador en equipo o haya implementado la controladora de red de uso las puertas de enlace con el conjunto.


-   **HNVv2**  

    Un número significativo de las nuevas características se incluye en HNVv2 que se implementa mediante el Azure Virtual filtrado de plataforma (VFP) extensión en el conmutador de Hyper-V de reenvío. HNVv2 está totalmente integrado con Microsoft Azure Stack, que incluye el nuevo controlador de red en la pila de redes definidas por Software (SDN).  Directiva de red virtual se define a través de Microsoft [controladora de red](../../../sdn/technologies/network-controller/Network-Controller.md) utilizando un RESTful NorthBound (NB) API y conectadas a un agente de Host a través de varios SouthBound interfaces (SBI) incluidos OVSDB. La directiva de programas de agente de Host en la extensión VFP del conmutador de Hyper-V donde se aplica.  

    > [!IMPORTANT]  
    > En este tema se centra en HNVv2.  

### <a name="VirtualNetworks"></a>Red virtual  

-   Cada red virtual se compone de uno o más subredes virtuales. Una red virtual forma un límite de aislamiento donde las máquinas virtuales dentro de una red virtual solo pueden comunicarse entre sí. Tradicionalmente, este aislamiento se aplica con VLAN 802.1q y de un intervalo de direcciones IP segregado etiqueta o identificador de VLAN. Pero, con HNV, aislamiento se aplica el uso de encapsulación de NVGRE o VXLAN para crear redes superpuestas con la posibilidad de que se superponen las subredes IP entre clientes o inquilinos.  

-   Cada red virtual tiene un único identificador de dominio de enrutamiento (RDID) en el host. Aproximadamente este RDID se asigna a un identificador de recurso para identificar el recurso de REST en la controladora de red de la red virtual. Se hace referencia a la recursos REST de red virtual con un espacio de nombres de identificador uniforme de recursos (URI) con el identificador de recurso anexados.  

### <a name="VirtualSubnets"></a>Subredes virtuales  

-   Una subred virtual implementa la semántica de la subred IP de capa 3 para las máquinas virtuales de la misma subred virtual. La subred virtual forms a un dominio de difusión (similar a una VLAN) y el aislamiento se aplica mediante el Id. de red de inquilinos de NVGRE (TNI) o el identificador de red VXLAN (VNI) el campo.  

-   Cada subred virtual pertenece a una única red virtual (RDID) y se asigna una subred Virtual único identificador (VSID) mediante el TNI o VNI clave del encabezado del paquete encapsulado. El VSID debe ser único dentro del centro de datos y se encuentra en el rango 4096 a 2 ^ 24-2.  

Una ventaja clave de la red virtual y el dominio de enrutamiento es que permite a los clientes llevar sus topologías de red (por ejemplo, subredes IP) a la nube. La Figura 2 muestra un ejemplo donde Contoso Corp cuenta con dos redes diferentes: Red de I+D y Red de ventas. Dado que estas redes cuentan con identificadores de dominio de enrutamiento diferentes, no pueden interactuar entre sí. Es decir, Red I+D de Contoso está aislada de Red de ventas de Contoso aunque ambas pertenezcan a Contoso Corp. Red I+D de Contoso contiene tres subredes virtuales. Tenga en cuenta que el RDID y el VSID son únicos dentro de un centro de datos.  

![Redes de cliente y subredes virtuales](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  

Figura 2: Redes de cliente y subredes virtuales  

**Reenvío de capa 2**  

En la figura 2, las máquinas virtuales en VSID 5001 pueden tener sus paquetes reenviados a las máquinas virtuales que se encuentran también en VSID 5001 a través del conmutador de Hyper-V. Los paquetes entrantes de una máquina virtual en VSID 5001 se envían a un VPort en el conmutador de Hyper-V específico. El conmutador de Hyper-V para estos paquetes se aplican las reglas de entrada (por ejemplo, la encapsulación) y las asignaciones (por ejemplo, el encabezado de encapsulación). Los paquetes se reenvían a un VPort diferentes en el conmutador de Hyper-V (si la máquina virtual de destino está conectada al mismo host) o a un conmutador Hyper-V en un host diferente (si la máquina virtual de destino se encuentra en un host diferente).  

**Capa 3 enrutamiento**  

De forma similar, las máquinas virtuales en VSID 5001 puede tener sus paquetes en el enrutador distribuido de HNV que está presente en VSwitch de cada host Hyper-V enrutados a máquinas virtuales en VSID 5002 o VSID 5003. Al entregar el paquete a la función Hyper-V switch, HNV actualiza el VSID del paquete entrante al VSID de la máquina virtual de destino. Esto solo sucederá si ambos VSID se encuentran en el mismo RDID.  Por lo tanto, los adaptadores de red virtual con RDID1 no pueden enviar paquetes a los adaptadores de red virtual con RDID2 sin atravesar una puerta de enlace.  

> [!NOTE]  
> En la descripción de flujo de paquete anterior, el término "máquina virtual" en realidad significa el adaptador de red virtual en la máquina virtual. El caso más común es que una máquina virtual contenga un solo adaptador de red virtual. En este caso, las palabras "máquina virtual" y "adaptador de red virtual" pueden significar conceptualmente lo mismo.  

Cada subred virtual define una subred de IP de capa 3 y un límite de dominio de difusión (L2) de capa 2 similar a una VLAN. Cuando una máquina virtual difunde un paquete, HNV usa replicación de unidifusión (UR) para realizar una copia del paquete original y reemplace la dirección IP de destino y MAC con las direcciones de cada máquina virtual que se encuentran en el mismo VSID.  

> [!NOTE]  
> Cuando se distribuya Windows Server 2016, se implementará multidifusiones de difusión y la subred mediante la replicación de unidifusión. No se admiten IGMP y enrutamiento de multidifusión entre subredes.  

Además de ser un dominio de difusión, el VSID proporciona aislamiento. Un adaptador de red virtual en HNV está conectado a un puerto de conmutador de Hyper-V que tendrá las reglas de ACL que se aplica directamente al puerto (virtualNetworkInterface recurso REST) o a la subred virtual (VSID) de los cuales es una parte.  

El puerto del conmutador de Hyper-V debe tener una regla de ACL que se aplica. Esta ACL podría ser permitir ALL, DENY ALL o ser más específico para permitir solo determinados tipos de tráfico basados en la coincidencia de 5-tupla (IP de origen, IP de destino, puerto de origen, puerto de destino, protocolo).  

> [!NOTE]  
> Las extensiones de conmutador de Hyper-V no funcionará con HNVv2 en la nueva pila de redes definidas por Software (SDN). HNVv2 se implementa mediante la extensión de conmutador de la plataforma de filtrado Virtual (VFP) de Azure que no se puede usar junto con cualquier otra extensión de conmutador 3rd-party.  

## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Conmutación y enrutamiento en virtualización de red de Hyper-V  
HNVv2 implementa la semántica de enrutamiento de nivel 3 (L3) funcione igual que un conmutador físico y cambio de nivel 2 (L2) correcto o enrutador funcionaría. Cuando una máquina virtual conectada a una red virtual de HNV intenta establecer una conexión con otra máquina virtual en la misma subred virtual (VSID) que primero deberá conocer la dirección MAC de la entidad emisora de certificados de la máquina virtual remota. Si hay una entrada ARP para la dirección IP de la máquina virtual de destino en la tabla ARP de la máquina virtual de origen, se usa la dirección MAC de esta entrada. Si no existe ninguna entrada, la máquina virtual de origen enviará que difusión ARP con una solicitud de la dirección MAC correspondiente a la dirección IP de la máquina virtual de destino que se va a devolver. El conmutador de Hyper-V se interceptar esta solicitud y envíela al agente de Host. El agente de Host buscará en su base de datos local para una dirección MAC correspondiente para la dirección IP de la máquina virtual de destino solicitado.  

> [!NOTE]  
> El agente de Host, que actúa como el servidor OVSDB, usa una variante del esquema VTEP para almacenar las asignaciones entre CA y PA, tabla de MAC y así sucesivamente.  

Si una dirección MAC está disponible, el agente de Host inserta una respuesta de ARP y lo envía a la máquina virtual. Después de la pila de red de la máquina virtual tiene toda la información de encabezado L2 necesaria, la trama se envía al puerto de Hyper-V correspondiente en el conmutador-V. Internamente, el conmutador de Hyper-V comprueba este marco en las reglas de coincidencia de tupla de N asignada al puerto-V y se aplica ciertas transformaciones al marco según estas reglas. Lo más importante, se aplica un conjunto de transformaciones de encapsulación para construir el encabezado de encapsulación mediante NVGRE o VXLAN, dependiendo de la directiva definida en la controladora de red. Según la directiva de programar el agente de Host, una asignación entre CA y PA se utiliza para determinar la dirección IP del host de Hyper-V en el que reside la máquina virtual de destino. El conmutador de Hyper-V garantiza que las reglas de enrutamiento correctas y se aplican las etiquetas VLAN para el paquete externo para que llegue a la dirección de PA remota.  

Si desea que una máquina virtual conectada a una red virtual de HNV crear una conexión con una máquina virtual en otra subred virtual (VSID), el paquete debe enrutarse en consecuencia. HNV se da por supuesto una topología de estrella donde hay una única dirección IP en el espacio de CA que se utiliza como el próximo salto para llegar a todos los prefijos IP (significado una ruta/puerta de enlace predeterminada). Actualmente, esto impone un límite para una única ruta predeterminada y no se admiten las rutas no predeterminadas.  

### <a name="routing-between-virtual-subnets"></a>Enrutamiento entre subredes virtuales  
En una red física, una subred IP es un dominio de nivel 2 (L2) donde los equipos (virtuales y físicos) pueden comunicarse directamente entre sí. El dominio de L2 es donde se aprenden las entradas de ARP (mapa de direcciones IP:MAC) a través de las solicitudes de ARP se difunden en todas las interfaces y las respuestas de ARP se envían al host que solicita un dominio de difusión. El equipo usa la información de direcciones MAC aprendida de la respuesta de ARP para construir completamente el marco de L2, incluidos los encabezados de Ethernet. Sin embargo, si una dirección IP está en una subred diferente de L3, la solicitud de ARP no cruza este límite L3. En su lugar, una interfaz de enrutador (puerta de enlace predeterminada o de próximo salto) L3 con una dirección IP en la subred de origen debe responder a estas solicitudes ARP con su propia dirección MAC.  

En redes de Windows estándar, un administrador puede crear rutas estáticas y asignarlos a una interfaz de red. Además, la "puerta de enlace predeterminada" se configura normalmente para que sea la dirección IP de próximo salto en una interfaz que se envían los paquetes destinados a la ruta predeterminada (0.0.0.0/0). Los paquetes se envían a esta puerta de enlace predeterminada si no hay rutas específicas existen. Normalmente se trata del enrutador de la red física.  HNV usa un enrutador integrado que forma parte de todos los hosts y tiene una interfaz en cada VSID para crear un enrutador distribuido para las redes virtuales.  

Puesto que HNV supone que la topología en estrella, el enrutador distribuido HNV actúa como puerta de enlace predeterminada única para todo el tráfico que circula entre subredes virtuales que forman parte de la misma red VSID. La dirección se usa como valores predeterminados de la puerta de enlace predeterminada a la dirección IP más baja en el VSID y se asigna al enrutador distribuido HNV. Este enrutador distribuido permite que una forma muy eficaz para todo el tráfico dentro de una red VSID se enrute adecuadamente ya que cada host puede enrutar el tráfico directamente al host correspondiente sin necesidad de intermediarios.  Esto es especialmente cierto cuando dos máquinas virtuales que se encuentran en la misma subred de VM, pero en diferentes subredes virtuales, están en el mismo host físico.  Como verá más adelante en esta sección, el paquete nunca debe abandonar el host físico.  

### <a name="routing-between-pa-subnets"></a>El enrutamiento entre subredes PA  
A diferencia de HNVv1 que asigna una dirección IP PA para cada subred Virtual (VSID), HNVv2 ahora usa una dirección IP de PA por miembro del equipo NIC Switch-Embedded Teaming (SET). La implementación predeterminada supone un equipo de dos NIC y asigna dos direcciones IP PA por host. Un único host tiene direcciones IP de PA asignadas desde la misma subred lógica de proveedor (PA) de la misma VLAN. De hecho se pueden encontrar dos máquinas virtuales de inquilino en la misma subred virtual en dos hosts diferentes que están conectados a dos subredes lógicas de proveedor diferente. HNV construirán los encabezados IP externos para el paquete encapsulado según la asignación entre CA y PA. Sin embargo, se basa en la pila de TCP/IP de host a ARP para la puerta de enlace de PA de forma predeterminada y, a continuación, genera los encabezados de Ethernet externos basados en la respuesta de ARP. Normalmente, esta respuesta ARP procede de la interfaz SVI en el conmutador físico o el enrutador de L3 donde el host está conectado. HNV, por tanto, se basa en el enrutador L3 para enrutar los paquetes encapsulados entre subredes lógicas de proveedor / VLAN.  

### <a name="routing-outside-a-virtual-network"></a>Enrutamiento fuera de una red virtual  
La mayoría de las implementaciones de clientes requerirán comunicación desde el entorno de HNV a los recursos que no forman parte de dicho entorno. Se requieren puertas de enlace de Virtualización de red para permitir la comunicación entre los dos entornos. Las infraestructuras que requieren una puerta de enlace de HNV incluyen la nube privada y nube híbrida. Básicamente, las puertas de enlace HNV son necesarios para el enrutamiento de nivel 3 entre redes (físicas) internas y externas (incluido NAT) o entre diferentes sitios o nubes (privadas o públicas) que utilizan un túnel VPN IPSec o GRE.  

Las puertas de enlace pueden venir en diferentes factores de forma físicos. Pueden generarse en Windows Server 2016, incorporan un conmutador de Rack superior (TOR) actúa como una puerta de enlace VXLAN, acceder a través de una IP Virtual (VIP) anunciada por un equilibrador de carga, colocarse en otros dispositivos de red existente, o puede ser un nuevo dispositivo de red independiente .  

Para obtener más información acerca de las opciones de puerta de enlace de RAS de Windows, consulte [puerta de enlace RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  

## <a name="packet-encapsulation"></a>Encapsulación de paquetes  
Cada adaptador de red virtual en HNV está asociado a dos direcciones IP:  

-   **Dirección de cliente** (CA) la dirección IP asignada por el cliente, en función de su infraestructura de intranet. Esta dirección permite al cliente intercambiar tráfico de red con la máquina virtual como si hubiera que no se ha movido a una nube pública o privada. La CA está visible para la máquina virtual y el cliente puede obtener acceso a ella.  

-   **Dirección de proveedor** (PA) la dirección IP asignada por el proveedor de hospedaje o los administradores de centros de datos en función de su infraestructura de red físico. La PA aparece en los paquetes de la red que se intercambian con el servidor que ejecuta Hyper-V que hospeda la máquina virtual. La PA está visible en la red física, pero no lo está para la máquina virtual.  

Las CA mantienen la topología de red del cliente, que está virtualizada y desacoplada de la topología y las direcciones de red física subyacente real, tal como lo implementan las PA. En el siguiente diagrama se muestra la relación conceptual entre las CA de máquinas virtuales y las PA de infraestructura de red como resultado de la virtualización de red.  

![Diagrama conceptual de virtualización de red a través de una infraestructura física](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  

Figura 6: Diagrama conceptual de virtualización de red a través de una infraestructura física  

En el diagrama, las máquinas virtuales de cliente envían paquetes de datos en el espacio de la entidad de certificación, que atraviesan la infraestructura de red física a través de sus propias redes virtuales, o "túneles". En el ejemplo anterior, los túneles pueden considerarse como "sobres" alrededor de los paquetes de datos de Contoso y Fabrikam con etiquetas verdes de envío (direcciones PA) que se entregan desde el host de origen a la izquierda al host de destino de la derecha. La clave es la forma en que los hosts determinan las "direcciones de envío" (PA) que corresponden a Contoso y Fabrikam de la CA, cómo se coloca el "sobres" alrededor de los paquetes y cómo los hosts de destino pueden desenvolver los paquetes y entregar a Contoso y Fabrikam destino de virtual machines correctamente.  

Esta simple comparación destacó los aspectos clave de la virtualización de red:  

-   La CA de cada máquina virtual se asigna a una PA de host físico. Puede haber varios CA asociados a la misma PA.  

-   Las máquinas virtuales envían paquetes de datos en los espacios de CA, que se colocan en un "sobre" con un par de origen y destino de PA basado en la asignación.  

-   Las asignaciones de CA-PA deben permitir a los hosts diferenciar paquetes de diferentes equipos virtuales del cliente.  

Como resultado, el mecanismo para virtualizar la red es virtualizar las direcciones de red que utilizan las máquinas virtuales. La controladora de red es responsable de la asignación de dirección y el agente de host mantiene la base de datos de asignación con el esquema MS_VTEP. En la próxima sección se describe el mecanismo real de virtualización de direcciones.  

## <a name="network-virtualization-through-address-virtualization"></a>Virtualización de red a través de virtualización de direcciones  
HNV implementa superposición redes de inquilinos mediante Network Virtualization Generic Routing Encapsulation (NVGRE) o el Virtual eXtensible Local Area Network (VXLAN).  VXLAN es el valor predeterminado.  

### <a name="virtual-extensible-local-area-network-vxlan"></a>Red de área Local virtual eXtensible (VXLAN)  
La Virtual eXtensible Local Area Network (VXLAN) ([7348 RFC](https://www.rfc-editor.org/info/rfc7348)) protocolo se ha adoptado ampliamente en el mercado, con soporte técnico de proveedores como Cisco, Brocade, Arista, Dell, HP y otros usuarios. El protocolo VXLAN usa UDP como transporte. El puerto de destino UDP asignado por IANA para VXLAN es 4789 y el puerto UDP de origen debe ser un valor hash de la información de los paquetes que se usará para la propagación de varias interior. Después del encabezado UDP, se anexa un encabezado VXLAN al paquete que incluye un campo reservado de 4 bytes seguido de un campo de 3 bytes para el VXLAN red identificador (VNI) - VSID - seguido de otro campo reservado de 1 byte. Después del encabezado VXLAN, se anexa el marco de L2 CA original (sin el marco de Ethernet de CA FCS).  

![Encabezado del paquete VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  

### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulación de enrutamiento genérico (NVGRE)  
Este mecanismo de virtualización de red usa la encapsulación de enrutamiento genérico (NVGRE) como parte del encabezado de túnel. En NVGRE, el paquete de la máquina virtual se encapsula dentro de otro paquete. El encabezado de este nuevo paquete tiene las direcciones IP PA de origen y de destino apropiadas, además del identificador de subred virtual, que se almacena en el campo Clave del encabezado GRE, como se muestra en la Figura 7.  

![Encapsulación NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  

Figura 7: Virtualización de red: encapsulación de NVGRE  

El identificador de subred Virtual permite a los hosts identificar la máquina virtual del cliente para cualquier paquete dado, aunque las PA y la entidad de certificación de los paquetes se pueden superponer. Esto permite a las máquinas virtuales del mismo host compartir una sola PA, como se muestra en la Figura 7.  

Compartir la PA tiene un gran impacto en la escalabilidad de la red. La cantidad de direcciones IP y MAC que la infraestructura de red necesita detectar puede reducirse sustancialmente. Por ejemplo, si cada host extremo tiene un promedio de 30 máquinas virtuales, la cantidad de direcciones IP y MAC que debe detectar la infraestructura de red se reduce en un factor de 30. Los identificadores de subred virtual insertados en los paquetes también permiten la fácil correlación de paquetes con los clientes reales.  

La PA de uso compartido de esquema para Windows Server 2012 R2 es una PA por VSID por host. Para Windows Server 2016, el esquema es una PA por miembro del equipo NIC.  

Con Windows Server 2016 y versiones posteriores, HNV es totalmente compatible con NVGRE y VXLAN de fábrica; no se requiere actualización ni la compra nuevo hardware de red como NIC (adaptadores de red), conmutadores o enrutadores. Esto es porque los paquetes en la conexión son el paquete IP común en el espacio de PA, que es compatible con la infraestructura de red de hoy en día.  Sin embargo obtener el mejor rendimiento, utilice admite NIC con los controladores más recientes que admiten las descargas de la tarea.  

## <a name="multi-tenant-deployment-example"></a>Ejemplo de implementación multiinquilino  
El siguiente diagrama muestra un ejemplo de implementación de dos clientes que se encuentra en un centro de datos en la nube con la relación entre CA y PA que definen las directivas de redes.  

![Ejemplo de implementación multiinquilino](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  

Figura 8: Ejemplo de implementación multiinquilino  

Considere el ejemplo en la Figura 8. Antes de pasar al servicio IaaS compartido del proveedor de hospedaje:  

-   Contoso Corp ejecutó un servidor SQL Server (llamado **SQL**) en la dirección IP 10.1.1.11 y un servidor web (llamado **Web**) en la dirección IP 10.1.1.12, que usa SQL Server para transacciones de bases de datos.  

-   Fabrikam Corp ejecutó un servidor SQL Server, también llamado **SQL** y asignó la dirección IP 10.1.1.11 y un servidor web, también llamado **Web** y también la dirección IP 10.1.1.12 que usa SQL Server para transacciones de bases de datos.  

Supondremos que el proveedor de servicios de hospedaje ha creado previamente la red lógica del proveedor (PA) a través de la controladora de red para que coincida con su topología de red física. La controladora de red asigna dos direcciones IP PA del prefijo de la lógica de la subred IP que los hosts están conectados. La controladora de red también indica la etiqueta a la VLAN adecuada para aplicar las direcciones IP.  

Con el controlador de red, Contoso Corp y Fabrikam Corp, a continuación, cree su red virtual y subredes que están respaldadas por la red lógica del proveedor (PA) especificada por el proveedor de servicios de hospedaje. Contoso Corp y Fabrikam Corp mueven sus respectivos servidores SQL Server y sus servidores web al mismo servicio IaaS compartido del proveedor de hospedaje donde, casualmente, ejecutan las máquinas virtuales **SQL** en el host 1 de Hyper-V y las máquinas virtuales (IIS7) **Web** en el host 2 de Hyper-V. Todas las máquinas virtuales conservan las direcciones IP de intranet originales (sus CA).  

Ambas compañías se asignan los siguientes Id. de subred Virtual (VSID) por la controladora de red como se indica a continuación.  El agente de Host en cada uno de los hosts de Hyper-V recibe las direcciones IP PA asignadas de la controladora de red y crea dos VNIC de host de PA en un compartimento de red no predeterminado. Una interfaz de red se asigna a cada uno de estos VNIC de host donde se asigna la dirección IP PA tal como se muestra a continuación:  

-   Máquinas virtuales de Contoso Corp VSID y PA: **VSID** es 5001, **SQL PA** es 192.168.1.10, **Web PA** es 192.168.2.20  

-   Máquinas virtuales de Fabrikam Corp VSID y PA: **VSID** es 6001, **SQL PA** es 192.168.1.10, **Web PA** es 192.168.2.20  

La controladora de red sondea todas las directivas de red (incluida la asignación entre CA y PA) para el agente de Host de SDN que mantendrá la directiva en un almacén persistente (en tablas de base de datos OVSDB).  

Cuando la máquina virtual de Contoso Corp Web (10.1.1.12) en el Host 2 de Hyper-V crea una conexión TCP con el servidor SQL Server en 10.1.1.11, sucede lo siguiente:  

-   ARP de máquina virtual para la dirección MAC de destino de 10.1.1.11  

-   La extensión VFP en el vSwitch intercepta este paquete y lo envía al agente de Host de SDN  

-   El agente de Host de SDN busca en su almacén de directivas para la dirección MAC para 10.1.1.11  

-   Si se encuentra un equipo MAC, el agente de Host inserta una respuesta de ARP a la máquina virtual  

-   Si no se encuentra un equipo MAC, no se envía ninguna respuesta y la entrada ARP en 10.1.1.11 de la máquina virtual se marca inaccesible.  

-   La máquina virtual ahora construye un paquete TCP con los encabezados IP y Ethernet de entidad emisora de certificados correctos y lo envía al vSwitch  

-   La extensión en el vSwitch de reenvío de VFP procesa este paquete a través de las capas VFP (descrito a continuación) asignada al puerto vSwitch de origen en el que se recibió el paquete y crea una nueva entrada de flujo en la tabla de flujo unificada VFP  

-   El motor VFP realiza la búsqueda de reglas de coincidencia o tabla de flujo para cada capa (por ejemplo, la capa de red virtual) basándose en los encabezados de dirección IP y Ethernet.  

-   La regla coincidente en el nivel de red virtual hace referencia a un espacio de la asignación entre CA y PA y realiza la encapsulación.  

-   El tipo de encapsulación (VXLAN o NVGRE) se especifica en la capa de red virtual, junto con el VSID.  

-   En el caso de encapsulación VXLAN, se construye un encabezado UDP externo con el VSID del 5001 en el encabezado VXLAN.  
    Se construye un encabezado IP externo con la dirección de PA de origen y de destino asignada para el Host 2 de Hyper-V (192.168.2.20) y el 1 de Host de Hyper-V (192.168.1.10), respectivamente, en función de almacén de directivas del agente de Host de SDN.  

-   Este paquete, a continuación, se pasa a la capa de enrutamiento de PA de VFP.  

-   La capa de enrutamiento de PA de VFP se referencia el compartimento de red utilizado para el tráfico de espacio de PA y un identificador de VLAN y utilizar la pila TCP/IP del host para reenviar el paquete de PA al Host de Hyper-V 1 correctamente.  

-   Tras la recepción del paquete encapsulado, Hyper-V Host 1 recibe el paquete en el compartimiento de red de PA y reenviarlo al vSwitch.  

-   El VFP procesa el paquete a través de sus capas VFP y crear una nueva entrada de flujo en la tabla de flujo unificado de VFP.  

-   El motor VFP coincide con las reglas ingres en el nivel de red virtual y quita los encabezados de Ethernet, IP y VXLAN del paquete encapsulado externa.  

-   El motor de VFP, a continuación, reenvía el paquete al puerto vSwitch al que está conectado la máquina virtual de destino.  

Un proceso similar para el tráfico entre el **sitio web** de Fabrikam Corp y las máquinas virtuales **SQL** usa la configuración de directiva HNV para Fabrikam Corp. Como resultado, con HNV, las máquinas virtuales de Fabrikam Corp y Contoso Corp interactúan como si estuvieran en sus intranets originales. Nunca pueden interactuar entre sí aunque usen las mismas direcciones IP.  

Las direcciones separadas (CA y PA), la configuración de directiva de los hosts de Hyper-V y la traducción de direcciones entre la entidad de certificación y la PA de tráfico entrante y saliente máquinas virtuales aíslan estos conjuntos de servidores mediante la clave de NVGRE o la VNID VLXAN. Además, las asignaciones de virtualización y la transformación desacoplan la arquitectura de la red virtual de la infraestructura de la red física. Si bien Contoso **SQL** y **Web** y Fabrikam **SQL** y **Web** residen en sus propias subredes IP CA (10.1.1/24), las implementaciones físicas se producen en dos hosts de diferentes subredes PA, 192.168.1/24 y 192.168.2/24, respectivamente. La implicación es que el aprovisionamiento de máquinas virtuales entre subredes y la migración en vivo son posibles con HNV.  

## <a name="hyper-v-network-virtualization-architecture"></a>Arquitectura de Virtualización de red de Hyper-V  
En Windows Server 2016, HNVv2 se implementa mediante el Azure Virtual filtrado de plataforma (VFP) que es una extensión de filtrado de NDIS en el conmutador de Hyper-V. El concepto clave de VFP es de un motor de flujo de la acción de la coincidencia con una API interna expuesto para el agente de Host de SDN para la programación de directiva de red. El agente de Host de SDN sí recibe la directiva de red de la controladora de red a través de los canales de comunicación OVSDB y WCF SouthBound. No sólo es la directiva de red virtual (por ejemplo, asignación entre CA y PA) programan mediante VFP pero directiva adicional, como las ACL, QoS y así sucesivamente.  

La jerarquía de objetos para la extensión de reenvío de VFP y el conmutador es el siguiente:  

-   vSwitch  

    -   Administración de NIC externa  

    -   Descargas de Hardware NIC  

    -   Reglas de enrutamiento globales  

    -   Puerto  

        -   Reenvío de capa de anclaje de selección precisa de salida  

        -   Listas de espacio para las asignaciones y los grupos NAT  

        -   Tabla de flujo unificada  

        -   Capa VFP  

            -   Tabla de flujo  

            -   Agrupar  

            -   Regla  

                -   Pueden hacer referencia a espacios de reglas  

En el programa VFP, una capa se crea por tipo de directiva (por ejemplo, la red Virtual) y es un conjunto genérico de las tablas de la regla y el flujo. No tiene ninguna funcionalidad intrínseco hasta que se asignan las reglas específicas para ese nivel para implementar esta funcionalidad. Cada capa se asigna una prioridad y las capas se asignan a un puerto por orden ascendente de prioridad. Las reglas se organizan en grupos que se basa principalmente en la dirección y la familia de direcciones IP. Grupos también tienen asignados una prioridad y a lo sumo, una regla de un grupo puede coincidir con un flujo determinado.  

La lógica de reenvío para el vSwitch con extensión VFP es como sigue:  

-   Procesamiento de entrada (entrada desde el punto de vista de paquete que entra en un puerto)  

-   Reenvío  

-   Procesamiento de salida (desde el punto de vista de paquete que salga de un puerto de salida)  

El VFP admite el reenvío de MAC interno para tipos de encapsulación de NVGRE y VXLAN, así como externo reenvío basada en VLAN de MAC.  

La extensión VFP tiene un acceso lento y la ruta de acceso rápida para el cruce seguro del paquete. El primer paquete en un flujo debe recorrer todos los grupos de reglas en cada capa y hacer una regla de búsqueda que es una operación costosa. Sin embargo, una vez que un flujo está registrado en la tabla de flujo unificada con una lista de acciones (según las reglas coincidas) todos los paquetes posteriores se procesarán según las entradas de la tabla de flujo unificada.  

La directiva de HNV se programa el agente de host. Cada adaptador de red de máquina virtual se configura con una dirección IPv4. Estas son las CA que utilizarán las máquinas virtuales para comunicarse entre sí y se pueden transportar en los paquetes IP desde las máquinas virtuales. HNV encapsula el marco de la entidad de certificación en un marco de PA según las directivas de virtualización de red almacenadas en la base de datos del agente de host.  

![Arquitectura HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  

Figura 9: Arquitectura HNV  

## <a name="summary"></a>Resumen  
Los centros de datos basados en la nube pueden proporcionar muchos beneficios como mayor escalabilidad y mejor utilización de los recursos. Advertir estos beneficios potenciales requiere una tecnología que fundamentalmente aborde los problemas de escalabilidad multiempresa en un entorno dinámico. HNV se diseñó para abordar estos problemas y también mejorar la eficacia operativa del centro de datos al desacoplar la topología de red virtual para la topología de red física. Creación de un estándar existente, HNV se ejecuta en el centro de datos actual y funciona con la infraestructura existente de VXLAN. Los clientes con HNV pueden ahora consolidar sus centros de datos en una nube privada o extender a la perfección sus centros de datos al entorno de un servidor proveedor de hospedaje con una nube híbrida.  

## <a name="BKMK_LINKS"></a>Vea también  
Para obtener más información acerca de HNVv2 vea los siguientes vínculos:  


|       Tipo de contenido       |                                                                                                                                              Referencias                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Recursos de la Comunidad**  |                                                                -   [Blog de arquitectura de nube privada](https://blogs.technet.com/b/privatecloud)<br />-Formular preguntas: [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)                                                                |
|         **RFC**          |                                                                   -   [RFC borrador de NVGRE](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                    |
| **Tecnologías relacionadas** | -Para detalles técnicos de virtualización de red de Hyper-V en Windows Server 2012 R2, consulte [detalles técnicos de virtualización de red de Hyper-V](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Controladora de red](../../../sdn/technologies/network-controller/Network-Controller.md) |

